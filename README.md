# weakjson
Weakjson is a JSON parser base on encoding/json. It provides a automatic type casting when unmarshalling JSON to golang structure.

Getting Started
===============

## Installing

Install Go and run `go get`:

```sh
$ go get -u github.com/remrain/weakjson
```

## Unmarshalling JSON to struct

Weakjson casting integer, string, float, bool automatic.

```go
package main

import (
    "fmt"
    "github.com/remrain/weakjson"
)

type Person struct {
    Name string
    Age  int 
}

func main() {
    json := []string{
        `{"name":"Janus", "age": 32}`,
        `{"name":"Janus", "age": "32"}`,
        `{"name":"Janus", "age": 32.5}`,
        `{"name":"Janus", "age": "32.5"}`,

        `{"name":"Casey", "age": false}`,
        `{"name":"Casey", "age": null}`,
        `{"name":"Casey", "age": 0}`,

        `{"name":"42", "age": 42}`,
        `{"name":42,   "age": 42}`,
    }   
    for _, v := range json {
        person := Person{}
        err := weakjson.Unmarshal([]byte(v), &person)
        if err != nil {
            fmt.Println(err)
        } else {
            fmt.Println(person)
        }   
    }   
}
```

This will print

```
{Janus 32}
...
{Casey 0}
...
{42 42}
...
```

No error will be occurred.

## Copy struct deeply

Copy struct via interface{}

```go
func Copy(dest, src interface{}) error {
    b, err := weakjson.Marshal(src)
    if err != nil {
        return err 
    }   
    err = weakjson.Unmarshal(b, dest)
    return err 
}

type (
    Person1 struct {
        Name string
        Age  int // age declared as `int`
    }   

    Person2 struct {
        Name string
        Age  string // age declared as `string`
    }   
)

func main() {
    person1 := Person1{"Maria", 21} 
    person2 := Person2{}
    Copy(&person2, &person1)
    fmt.Println(person2)
}
```

## Contact
[RemRain](cy@remrain.com)

## License
Apache License Version 2
