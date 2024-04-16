---
tags:
  - Go
  - Wizardry
date: 2024-03-20T23:19
title: Go Wizardry Ultimate
---
<!-- 2024-03-20-2319 (March 20, 2023, 11:19 PM) -->

# Go Wizardry Ultimate Guide


## Go Basics
```go
go mod init <name of module> 
/* <name of module> 
- make the project available to the public if is a repo link
*/
```

### `main.go`
- The entry point of the application (required to have this file)
- basic structure:
```go
package main

import (
  "fmt"
)

func main() {
  name := "Ej"
  fmt.Printf("Hello, %s!", name)
}
```

- go projects by design can have multiple `main.go` file (multiple entry points) 
- AS LONG AS: they are different directories
```go
|- main.go
|- go.mod
| variables/
| |- main.go
```

### Go can infer types
```go
var name = "Ej" // inferred as string
var name string = "Ej" // explicit

name := "Ej" // preferred way: declare and assign shorthand

// can also do
var name string
name = "Ej"
```

**declared variables that are not used run on zero values**
- gotchas: NOT ALL data structures are like this in Go

### Arrays in Go
- fixed in size (of course)
```go
package main
func main() {
  // 1
  name := [3]string{"Ej", "D", "T"}

  // 2
  name := [3]string{}

  name[0] = "Ej"
  name[1] = "D"
  name[2] = "T"

  // 3
  var names [3]string
  names[0] = "Ej"
  names[1] = "D"
  names[2] = "T"

  fmt.Println(names)
}
```

### Slices in Go
- dynamic arrays (list)
- is a struct:
```go
type slice struct {
  array unsafe.Pointer
  len int // is the number of existing elements
  cap int // is the max size
}
```
- example:
```go
package main
func main() {
  animals := []string{"dog", "cat", "bird"}

  animals = append(animals, "fish")

  animals = slices.Delete(animals, 2)
  animals = slices.Delete(animals, 0, 2)
}
```

### Loops in Go
- all for written as `for` loops but same concept with all other loops applies
```go

// classic for loop
for i := 0; i < 5; i++ {
  fmt.Println(i)
}

for i := 0; i < len(animals); i++ {
  fmt.Println(animals[i])
}

// go-esque way (clean af)
for index, value := range animals {
  fmt.Println(index, value)
  fmt.Printf("this is my index %d and this is my animal %d\n", index, value)
}

for value := range 10 {}
  fmt.Println(value)
}

// the "while" loop in Go
i := 0
for i < 5 {
  fmt.Println(i)
  i++
}

// if i is out of range, then it will panic (will not even compile)
```

### Structs in Go
- favor composition over inheritance
- structs in Go are passed by value (not by reference) by default
  - to pass by reference, use pointers
```go
package main

import (
  "fmt"
)

type Person struct {
  Name string
  Age int
}

func NewPerson(name string, age int) Person {
  Name: name,
  Age: age,
}

func NewPerson2(name string, age int) &Person {
  return &Person{
    Name: name,
    Age: age,
  }
}

// will still pass by value (doesn't modify the original struct)
func (p Person) changeNamePassByVal(newName string) {
  p.Name = newName
}

// will now pass by reference (modifies the original struct)
// --> using a pointer
func (p *Person) changeNamePassByRef(newName string) {
  p.Name = newName
}

func main() {
  // this won't error
  // --> Age will automatically be 0 (zero-value default, when variables are declared but not initialized)
  myPersonWhat := Person{
    Name: "Won't Error",
  }

  // creates a new person
  myPerson := Person{
    Name: "Ej",
    Age: 20,
  }

  myPerson.Name = "Edwin"
  myPerson.Age = 21
  myPerson.changeNameByVal("Hehe") // won't modify the original struct

  fmt.Printf("My name is %s and I am %d years old", myPerson.Name, myPerson.Age)
  // --> results to: My name is Edwin and I am 21 years old

  myPerson.changeNameByRef("NewEj") // modifies the original struct
  // --> results to: My name is NewEj and I am 21 years old

  a := 7
  b := &a
  fmt.Println(a) // 7

  *b = 9 // deference the pointer (access the value in the addr of a)
  fmt.Println(*b) // 9
  fmt.Println(a) // 9


  fmt.Printf("My name is %s and I am %d years old", myPerson.Name, myPerson.Age)
}
```

### Imports in Go
- public functions are capitalized (start with a capital letter)
- private functions are lowercase (start with a lowercase letter)
```go
package main
import (
  "fmt"
  "<module-name>/<package-or-directory-name>"
  // can also do this to avoid collisions
  xyzName "module-name/package-or-directory-name"
)
func main() {
  fmt.Println("Hello, World!")
}
```

### Exercise:
```md
- Create a Circle struct with a single field radius of type float64 
- Define a circumference method on the circle:
  - C = 2*pi*r
  - pi = 3.14
- Bonus
  - add a method for area
  - A = pi*r^2
```

```go
package main

import (
  "fmt"
  "math"
)

const (
  PI = 3.14
)

type Circle struct {
  radius float64 
}

func (c *Circle) circumference(radius float64) float64{
  circumference := 2 * PI * radius
  return circumference
}

// or with just printing
// --> notice (c Circle) is not (c *Circle), this is because we are just printing it, and we do not return anything
func (c Circle) circumferencePrint(radius float64) {
  circumference := 2 * PI * radius
  fmt.Printf("%f\n", circumference)
}

func (c *Circle) area(radius float64) float64{
  // area := PI * radius**2
  area := PI * math.Sqrt(radius)
  return area
}

// creates a another copy of struct Circle
func NewCircle(radius float64) Circle {
  return Circle{
    radius: radius,
  }
}

func main() {
  circle := Circle{
    radius: 5
  }

  fmt.Printf("%f\n", circle.circumference(circle.radius))
  fmt.Printf("%f\n", circle.area(circle.radius))
}
```

## AWS
- cloud lead
- biggest market share
- many services
- mainstream
- practical

### Infrastructure as Code (IaC)
1. Terraform
2. Pulumi
3. AWS CDK (Cloud Development Kit)
4. Bicep (Azure)

Setup AWS here: https://gist.github.com/dtauer/a35f5c50c8e3f85ec827e30a97dd7019

Using CDK:
```bash
cdk init app --language go
```

`go_cdk.go` - main file
- run `go get` to install the dependencies
- inside:
  - `jsii` - important when using different language (other than typescript)
    --> allows us to use languages other than typescript
    --> transpiles to typescript

### creating a stack

1. Lambda
- NOTE: tagging in structs (to be unmarshalled) and errors are values in Go: 
```go
package main

type myEvent struct {
  Username string `json:username`
}

func HandleRequest(event myEvent) error {
  if event.Username == "" {
    // panic("Oh no! Username cannot be empty")
    return "", fmt.Errorf("Username cannot be empty")
  }

    return fmt.Sprintf("Successfully called by - %s", event.Username) ,nil
}

func main() {

}

```


## Resources
- [Go Class in DETAIL](https://www.youtube.com/playlist?app=desktop&list=PLoILbKo9rG3skRCj37Kn5Zj803hhiuRK6&si=2Rn65jJzEayIKqN4)
- [Complete Go Notes](https://docs.google.com/document/d/1Zb9GCWPKeEJ4Dyn2TkT-O3wJ8AFc-IMxZzTugNCjr-8/edit)
- good intro by Melkey (from Frontend Masters): [Build Go Apps That Scale on AWS]() 
  - [GitHub Repo]()
### Websites
- [Effective Go](https://go.dev/doc/effective_go/)
- [Go by Example](https://gobyexample.com)
- [Official Go Tour](https://go.dev/tour/)
- [Learn Go With Tests](https://quii.gitbook.io/learn-go-with-tests/go-fundamentals/install-go)

### Books
- The Go Programming Language by Donovan Kernighan
- Let's Go & Let's Go Further by Alex Edwards

### Conference Talks
- Rob Pike - [Go Concurrency Patterns](https://www.youtube.com/watch?v=f6kdp27TYZs)
- Rob Pike Conference Talks
