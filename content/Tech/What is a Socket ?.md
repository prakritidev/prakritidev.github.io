---
title: '"What is a Socket ?"'
draft: 
tags:
  - socket
---
### What is a Socket?

A **socket** is a communication endpoint used in networking to send and receive data between two devices (often referred to as a client and a server) over a network, such as the internet. A socket is identified by an IP address and a port number.

#### Key Concepts:
- **IP Address**: A unique address that identifies a device on a network.
- **Port Number**: A numerical identifier within the IP address that distinguishes different services or applications on the same device.
- **TCP/UDP**: Sockets can use either the TCP (Transmission Control Protocol) for reliable, connection-oriented communication or UDP (User Datagram Protocol) for faster, connectionless communication.

### Use Cases for Sockets

1. **Web Servers and Clients**:
   - Sockets are used in web servers to listen for incoming connections from clients (such as browsers) and in clients to initiate connections to servers.

2. **Real-Time Communication**:
   - Applications like chat servers, VoIP (Voice over IP), and online gaming use sockets to handle real-time data transmission between users.

3. **File Transfer**:
   - Protocols like FTP (File Transfer Protocol) use sockets to transfer files between a client and a server.

4. **Distributed Systems**:
   - Sockets facilitate communication between different components of a distributed system, such as microservices.

5. **IoT Devices**:
   - Internet of Things (IoT) devices use sockets to communicate with servers or other devices for data collection and control.

### Simple Socket Application in Go (Golang)

Let's create a basic client-server application using sockets in Go.

#### Server Code (TCP Echo Server)

The server will listen on a port, accept incoming connections, and echo back any data it receives from the client.

```go
package main

import (
	"bufio"
	"fmt"
	"net"
	"os"
)

func main() {
	// Listen on a port
	ln, err := net.Listen("tcp", ":8080")
	if err != nil {
		fmt.Println("Error starting the server:", err)
		return
	}
	defer ln.Close()
	fmt.Println("Server started and listening on port 8080")

	for {
		// Accept a connection
		conn, err := ln.Accept()
		if err != nil {
			fmt.Println("Error accepting connection:", err)
			continue
		}
		fmt.Println("Client connected")

		// Handle the connection in a new goroutine
		go handleConnection(conn)
	}
}

func handleConnection(conn net.Conn) {
	defer conn.Close()

	// Read data from the connection
	reader := bufio.NewReader(conn)
	for {
		message, err := reader.ReadString('\n')
		if err != nil {
			fmt.Println("Error reading from connection:", err)
			return
		}

		// Print the received message
		fmt.Print("Received message: ", message)

		// Echo the message back to the client
		_, err = conn.Write([]byte(message))
		if err != nil {
			fmt.Println("Error writing to connection:", err)
			return
		}
	}
}
```

#### Client Code (TCP Client)

The client will connect to the server, send a message, and display the server's response.

```go
package main

import (
	"bufio"
	"fmt"
	"net"
	"os"
)

func main() {
	// Connect to the server
	conn, err := net.Dial("tcp", "localhost:8080")
	if err != nil {
		fmt.Println("Error connecting to server:", err)
		return
	}
	defer conn.Close()

	// Read input from the user
	reader := bufio.NewReader(os.Stdin)
	fmt.Print("Enter a message: ")
	message, _ := reader.ReadString('\n')

	// Send the message to the server
	_, err = conn.Write([]byte(message))
	if err != nil {
		fmt.Println("Error sending message:", err)
		return
	}

	// Receive the response from the server
	response, err := bufio.NewReader(conn).ReadString('\n')
	if err != nil {
		fmt.Println("Error reading response:", err)
		return
	}

	// Print the server's response
	fmt.Println("Server response:", response)
}
```

### How to Run the Application

1. **Run the Server**:
   - Save the server code in a file called `server.go`.
   - Run the server using the command: `go run server.go`.
   - The server will start and listen for incoming connections on port 8080.

2. **Run the Client**:
   - Save the client code in a file called `client.go`.
   - Run the client using the command: `go run client.go`.
   - Enter a message when prompted. The client will send the message to the server, and the server will echo it back.
