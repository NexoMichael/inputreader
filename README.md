# Golang implementation of "getchar" function which allows you to intercept entered symbols right after key presses.

[![BuildStatus Widget]][BuildStatus Result]
[![CodeCovWidget]][CodeCovResult]
[![GoReport Widget]][GoReport Status]
[![GoDoc Widget]][GoDoc Link]

[BuildStatus Result]: https://travis-ci.org/NexoMichael/inputreader
[BuildStatus Widget]: https://travis-ci.org/NexoMichael/inputreader.svg?branch=master

[GoReport Status]: https://goreportcard.com/report/github.com/NexoMichael/inputreader
[GoReport Widget]: https://goreportcard.com/badge/github.com/NexoMichael/inputreader

[CodeCovResult]: https://coveralls.io/github/NexoMichael/inputreader
[CodeCovWidget]: https://coveralls.io/repos/github/NexoMichael/inputreader/badge.svg?branch=master

[GoDoc Link]: http://godoc.org/github.com/NexoMichael/inputreader
[GoDoc Widget]: http://godoc.org/github.com/NexoMichael/inputreader?status.svg

## Installation

Use the `go` command:

    go get github.com/NexoMichael/inputreader

## Usage

    package main

    import (
        "fmt"
        "os"
        "syscall"

        input "github.com/NexoMichael/inputreader"
    )

    func main() {
        reader, _ := input.NewInputReader(os.Stdin)
        defer func() {
            reader.Close()
        }()

        var b [1]byte
        for {
            n, err := reader.Read(b[:])
            if err != nil || n == 0 {
                return
            }

            fmt.Printf(" - code is %d\n\r", b[0])

            switch syscall.Signal(b[0]) {
            case syscall.SIGQUIT, syscall.SIGTERM, syscall.SIGINT:
                return
            }
        }
    }

## Requirements

inputreader package requires Go >= 1.5.

## Copyright

Copyright (C) 2018 by Mikhail Kochegarov <mikhail@kochegarov.biz>.

UUID package releaed under MIT License.
See [LICENSE](https://github.com/NexoMichael/inputreader/blob/master/LICENSE) for details.