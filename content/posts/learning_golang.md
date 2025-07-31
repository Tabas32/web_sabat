+++ 
draft = false
date = 2025-07-31T21:19:36+02:00
title = "Notes from learning golang"
description = "These are my notes for learning golang, in unstructured form."
slug = ""
authors = [ "Marian Sabat" ]
tags = [ "programming", "golang" ]
categories = []
externalLink = ""
series = []
+++

## Project

To create module (go.mod) run `go mod init <name>`  
To get dependencies to project run `go mod tidy`  
To install (download) package run `go install <package>@latest` or any version  

Search packages at 'https://pkg.go.dev/'

compilation: `go build -o output/path/app src/`

## Switch

```go
const A int8 = 25

// switch with input variable
switch A {
	case 10:
		fmt.Println("A is 10")
	case 20:
		fmt.Println("A is 20")
	default:
		fmt.Println("A is something else")
}

// switch that can use any bool statements as case
switch {
	case A > 10:
		fmt.Println("A is more than 10")
	case A <= 10:
		fmt.Println("A is less than 10")
}

// switch with 2 expresions as input
switch B := 30; B {
	case 10:
		fmt.Println("B is 10")
	case 20:
		fmt.Println("B is 20")
}

```

## Http client

```go
import (
	"net/http"
	"time"
	"fmt"
	"io"
)

func TestHttpClient() {
	// Create client
	client := http.Client { Timeout: time.Second * 10 }

	// Create request
	req, err := http.NewRequest("GET", "http://www.google.com", nil)

	if err != nil {
		fmt.Println("Error creating request", err)
	}

	// Call request
	response, err := client.Do(req)

	if err != nil {
		fmt.Println("Error sending request", err)
	}

	// You need to close Body, otherwise connections stay open
	defer response.Body.Close()

	// Read body
	body, err := io.ReadAll(response.Body)

	if err != nil {
		fmt.Println("Error reading body", err)
	}

	fmt.Println(body)
}
```
