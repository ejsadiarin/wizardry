,---
date: 2024-12-27T20:25
tags: 
- Go
---
<!-- 2024-12-27-2025 (December 27, 2024 08:25:29 PM) -->

# Difference between `io.ReadAll()`, `bufio.NewReader()/bufio.Reader`, and `net.Conn.Read()`

* `io.ReadAll()` 
    - function reads data until an `EOF` marker is encountered
    
* `bufio.NewReader()/bufio.Reader`, and `net.Conn.Read()`
    - reads fixed number of bytes (up to buffer size) and doesn't wait for an `EOF` 
    - suitable for HTTP connections, where connection is needed to be kept alive
    - uses a buffer to store data sent from the clients

# `curl` behavior

- by default `curl` uses a default header: `Connection: keep-alive`

- because the connection isn't closed by `curl`, `io.ReadAll` keeps waiting for more data and doesn't return,
    - causing the server to hang and preventing the response from being sent.

- so when doing `curl -i http://localhost:<port>/` (NOTE that `-i` means to include headers)
    - `io.ReadAll()`: doesn't return since it waits for an EOF marker intending to `read all` from the stream 
    - `bufio.Reader` / `net.Conn.Read()`: returns as soon as the data in the buffer is available, and DOESN'T WAIT for an `EOF` marker

# Go code example

```go
package main

import (
	"fmt"
	"net"
	"os"
	"strings"
)

// Ensures gofmt doesn't remove the "net" and "os" imports above (feel free to remove this!)
var (
	_ = net.Listen
	_ = os.Exit
)

func main() {
	// You can use print statements as follows for debugging, they'll be visible when running tests.
	fmt.Println("Logs from your program will appear here!")

	// Uncomment this block to pass the first stage

	l, err := net.Listen("tcp", "0.0.0.0:4221")
	if err != nil {
		fmt.Println("Failed to bind to port 4221")
		os.Exit(1)
	}

	conn, err := l.Accept()
	if err != nil {
		fmt.Println("Error accepting connection: ", err.Error())
		os.Exit(1)
	}
	defer conn.Close()

	// extract URL path from request
	req := make([]byte, 1024) // 1. create buffer of size 1024 bytes
	conn.Read(req) // 2. read incoming request from the connection and store it in buffer
	fmt.Println(string(req)) // print raw request
	var response string
	if !strings.HasPrefix(string(req), "GET / HTTP/1.1") { // 3. if the header from the request has invalid URL path then return 404 Not Found
		response = "HTTP/1.1 404 Not Found\r\n\r\n" // NOTE: the \r\n\r\n is the CRLF
		conn.Close()
		return
	}

	response = "HTTP/1.1 200 OK\r\n\r\n"
	_, err = conn.Write([]byte(response)) // 4. otherwise (valid "/" path), return 200 OK  
	if err != nil {
		fmt.Println("Error writing data", err.Error())
		os.Exit(1)
	}
}
```
```
```
