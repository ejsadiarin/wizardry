---
title: Go Networking Wizardry
date: 2024-02-29-2223 (February 29, 2024 10:23 PM)
tags: 
- Go
- Leap Year btw
- Networking
- Wizardry
---

# Go Networking Wizardry - The Only Practical Guide You'll Ever Need
the `net`

- simple http web server:
```go
package main
import (
    "fmt"
    "net/http"
)
func helloHandler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "Hello, World!")
}
func main() {
    http.HandleFunc("/", helloHandler)
    fmt.Println("Starting server on port 8080...")
    http.ListenAndServe(":8080", nil)
}
```

### Features of Go's Networking Library
1. **TCP/IP Networking**
- The `net` package for TCP/IP networking, including creating servers and clients.
2. **UDP Networking**
- The`net` package with support for creating UDP servers and clients.
3. **HTTP Server and Client**
- The `net/http` package allows for building HTTP servers and clients
- perfect for creating web applications and services.
4. **WebSockets**
- For real-time communication, Go supports WebSockets through the `net/http` package.
5. **TLS/SSL**
- Secure communication can be implemented using TLS/SSL, with Go's `crypto/tls` package.
6. **DNS Resolution**
- The `net` package also includes functionalities for DNS
 resolution.
7. **IP Address Manipulation**
- The `net` package for parsing and manipulating IP addresses.
