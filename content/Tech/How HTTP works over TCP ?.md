---
title: '"How HTTP works over TCP ?"'
draft: 
tags:
  - http
  - tcp
---
### Establishing a TCP Connection

#### Overview
TCP (Transmission Control Protocol) is a core protocol of the Internet Protocol Suite. It establishes a reliable, ordered, and error-checked connection between a client (e.g., your browser) and a server (e.g., google.com). This process involves a series of steps to ensure that both parties are ready to send and receive data.

#### Step-by-Step Process

##### a. **Socket Creation**
- **What is a Socket?**
  - A socket is an endpoint for communication between two machines. It includes an IP address and a port number, which together allow communication over a network.
- **Client Socket**:
  - The client (your device) creates a socket and binds it to a local port number. This socket will be used to communicate with the server.
- **Server Socket**:
  - The server also has a socket open on a specific port (e.g., port 80 for HTTP, port 443 for HTTPS), ready to accept incoming connections.

##### b. **Three-Way Handshake**
The three-way handshake is a process used to establish a TCP connection. It ensures that both the client and server are ready to transmit data and that they agree on the initial sequence numbers.

- **Step 1: SYN (Synchronize)**
  - The client initiates the connection by sending a TCP segment with the SYN flag set to the server. This packet contains:
    - **SYN Flag**: Indicates the desire to establish a connection.
    - **Initial Sequence Number (ISN)**: A randomly generated number used to track the sequence of transmitted bytes.

- **Step 2: SYN-ACK (Synchronize-Acknowledge)**
  - The server responds with a TCP segment that has both SYN and ACK flags set. This packet contains:
    - **SYN Flag**: Indicates the server's agreement to establish a connection.
    - **ACK Flag**: Acknowledges the receipt of the client’s SYN packet by setting the acknowledgment number to the client’s ISN + 1.
    - **Server’s Initial Sequence Number (ISN)**: The server also generates a random ISN to track its sequence of bytes.

- **Step 3: ACK (Acknowledge)**
  - The client sends a final ACK packet back to the server. This packet contains:
    - **ACK Flag**: Acknowledges the server’s SYN-ACK packet by setting the acknowledgment number to the server’s ISN + 1.
  - With this step, the connection is established, and both client and server have agreed on each other’s initial sequence numbers.

##### c. **Connection Established**
- **Data Transmission Begins**:
  - After the handshake, the connection is fully established, and data can be transmitted. TCP ensures that the data is split into segments, transmitted reliably, and reassembled in the correct order at the destination.
- **Error Checking and Flow Control**:
  - TCP provides mechanisms for error checking, retransmission of lost segments, and flow control to ensure that data is sent at a rate the receiver can handle.

##### d. **Connection Maintenance**
- **Keep-Alive Mechanism**:
  - TCP connections may be kept alive if there is a need for multiple requests and responses without re-establishing the connection each time.
- **Timeouts and Retransmission**:
  - If segments are lost or corrupted, TCP automatically retransmits them. If a connection is idle for too long, it may be closed to free up resources.

---

## HTTP 
HTTP (Hypertext Transfer Protocol) is the foundation of data communication on the web. It defines the structure of requests sent by a client (such as a browser or `curl`) to a server and the structure of responses sent back to the client. 

#### Step-by-Step Process

##### a. **HTTP Request Line**
- **Request Method**:
  - The first part of the request line specifies the HTTP method. Common methods include:
    - `GET`: Requests data from the server (e.g., a webpage).
    - `POST`: Submits data to the server (e.g., form submission).
    - `PUT`: Updates data on the server.
    - `DELETE`: Removes data from the server.
  - Example: `GET / HTTP/1.1`

- **Request Path**:
  - The path specifies the resource being requested. For example, `/` refers to the root of the website, while `/about` might refer to an "About" page.
  - Example: `GET /about HTTP/1.1`

- **HTTP Version**:
  - Specifies the version of the HTTP protocol being used. Common versions include `HTTP/1.1`, `HTTP/2`, and `HTTP/3`.
  - Example: `GET /about HTTP/1.1`

##### b. **HTTP Headers**
- **Purpose**:
  - Headers provide additional information about the request, such as the format of the data being sent, the client making the request, and instructions for handling the connection.

- **Common Headers**:
  - `Host`: Specifies the domain name of the server (e.g., `Host: google.com`).
  - `User-Agent`: Identifies the client software (e.g., browser or `curl`).
  - `Accept`: Indicates the MIME types the client can handle (e.g., `Accept: text/html`).
  - `Connection`: Controls whether the connection should be kept alive (e.g., `Connection: keep-alive`).
  - `Content-Type`: Specifies the media type of the request body (e.g., `Content-Type: application/json` for JSON data in a `POST` request).

- **Example**:
  ```
  GET / HTTP/1.1
  Host: google.com
  User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/92.0.4515.107 Safari/537.36
  Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8
  Connection: keep-alive
  ```

##### c. **HTTP Request Body (Optional)**
- **Purpose**:
  - The body of the request contains data that is being sent to the server. This is typically used with methods like `POST`, `PUT`, or `PATCH`.

- **Content Types**:
  - Depending on the `Content-Type` header, the body could be:
    - **Form Data**: `application/x-www-form-urlencoded`
    - **JSON**: `application/json`
    - **XML**: `application/xml`
    - **Binary Data**: For file uploads or images.

- **Example**:
  - For a `POST` request submitting form data:
    ```
    POST /login HTTP/1.1
    Host: example.com
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 32

    username=user&password=pass123
    ```

##### d. **HTTPS (TLS/SSL)**
- **Purpose**:
  - When using HTTPS, the communication between the client and server is encrypted using TLS (Transport Layer Security), providing confidentiality and integrity.

- **TLS Handshake**:
  - Before the HTTP request is sent, the client and server perform a TLS handshake:
    - **ClientHello**: The client sends a list of supported cryptographic algorithms and the highest TLS version it supports.
    - **ServerHello**: The server responds with its chosen algorithm, a certificate, and a request for a client certificate if necessary.
    - **Key Exchange**: Both parties exchange cryptographic keys to establish a shared secret.
    - **Session Established**: All further communication is encrypted using the session keys.

- **Secure HTTP Request**:
  - Once the TLS session is established, the HTTP request is encrypted and sent to the server. The server decrypts the request, processes it, and encrypts the response before sending it back to the client.

##### e. **Response Handling**
- **Receiving the Response**:
  - After sending the HTTP request, the client waits for the server's response, which will include the HTTP status code, headers, and optionally, a response body (e.g., HTML, JSON).

- **Processing the Response**:
  - The client processes the response headers to determine the content type, status, and any cookies or caching instructions. The response body is then parsed and displayed or used by the client.

##### f. **Connection Closure**
- **Persistent Connections**:
  - If the `Connection: keep-alive` header is used, the TCP connection remains open for additional requests, reducing latency for subsequent requests.
  
- **Connection Termination**:
  - Once all requests and responses are completed, the connection may be closed using a TCP **four-way handshake**:
    - **FIN (Finish)**: The client sends a FIN packet to signal the end of the communication.
    - **ACK**: The server acknowledges the FIN packet.
    - **FIN**: The server sends its own FIN packet.
    - **ACK**: The client acknowledges the server’s FIN, fully closing the connection.
