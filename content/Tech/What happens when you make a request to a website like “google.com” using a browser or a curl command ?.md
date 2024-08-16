---
title: '"What happens when you make a request to a website like “google.com” using a browser or a curl command ?"'
draft: 
tags:
  - http
  - browser
  - internet
  - dns
aliases:
---
# DNS

DNS (Domain Name Resolver) is responsible for resolving the names to IP address to identify each other on the internet. 

```
curl https://www.lewdstuff.com
```

However not every request goes to DNS all the time. All the things are cached at multiple levels.

#### Ckecking the Cache

- Local Cache: The OS maintains a DNS cache where it stires the recent resolved domain names.
- Browser Cache: They also have entry, if local cache is missed the brwoser is used. 
- HostFile: Before querying the DNS server, the system checks the hosts file (e.g., /etc/hosts on Linux/macOS or C:\Windows\System32\drivers\etc\hosts on Windows). If there’s a matching entry, it’s used.

#### Recurvsize DNS Query

- If the cache does not have the IP address, the query is sent to the **Recursive DNS Resolver** (usually provided by your ISP or a third-party like Google’s 8.8.8.8).
- The recursive resolver queries the **Root DNS Server** (one of 13 distributed around the world) for the Top-Level Domain (TLD) server (e.g., .com).
- The TLD server directs the resolver to the **Authoritative DNS Server** for the domain (e.g., the server responsible for google.com).
- The authoritative DNS server responds with the IP address for “google.com”.
- The IP address is returned to your computer, which caches it for future use.
- The TTL (Time to Live) value in the DNS response determines how long the result can be cached.

For deep dive check this note [[Recursive DNS]].


#### The Connection Establishement. 

TCP is used for communication between client and server. 

How does this work ?

##### Socket Creation: 

The OS creates a socket that is binded to a local port. You can create that using bash as well.
```
#!/bin/bash

HOST="www.example.com"
PORT=80

# Create a TCP socket
exec 3<>/dev/tcp/${HOST}/${PORT}

# Send data to the socket
echo -e "GET / HTTP/1.1\r\nHost: ${HOST}\r\n\r\n" >&3

# Read data from the socket
while read -r line <&3; do
    echo "$line"
done

# Close the socket
exec 3<&-
exec 3>&-
```
###### For Deep Dive [[What is a Socket ?]]
##### Three way HandShake: 
**SYN (Synchronize)**: Your computer sends a SYN packet to the server’s IP address, specifying a random sequence number and requesting a connection on a specific port (e.g., port 80 for HTTP or 443 for HTTPS).

**SYN-ACK (Synchronize-Acknowledge)**: The server responds with a SYN-ACK packet, acknowledging the client’s sequence number and sending its own sequence number.

**ACK (Acknowledge)**: Your computer sends an ACK packet back to the server, acknowledging the server’s sequence number. The TCP connection is now established.

With the connection established, data can now be reliably sent between client and server. TCP handles sequencing, error detection, and retransmission of lost packets.

## Sending HTTO Request

1. **HTTP Request Line**:

The request starts with a line like GET / HTTP/1.1, where:

GET is the HTTP method. 
/ is the path to the resource (in this case, the homepage).
HTTP/1.1 specifies the version of the HTTP protocol.

2. **Request Headers**:

Headers provide additional information about the request, such as:
Host: google.com: Specifies the domain being requested.
User-Agent: ...: Identifies the client making the request (browser, curl, etc.).
Accept: text/html,...: Indicates the types of responses the client can handle.
Connection: keep-alive: Suggests that the connection should remain open for further requests.

3. **Request Body (Optional)**:

For methods like POST, the request includes a body with data (e.g., form data, JSON).

4. **HTTPS (TLS/SSL)**

If the request is over HTTPS, before the HTTP request is sent, the client and server perform a **TLS (Transport Layer Security)** handshake:
**ClientHello**: The client sends a list of supported cipher suites and the highest TLS version it supports.
**ServerHello**: The server chooses a cipher suite, sends its certificate, and may request a client certificate.
**Key Exchange**: Both parties exchange cryptographic keys to establish a secure connection.
**Secure Session**: From now on, all communication is encrypted.

##  Processing the Request on the Server

Once the server receives the HTTP request, it processes it to generate a response. This step involves various server-side operations, depending on the request.  

1. **Request Handling**:

The web server (e.g., Nginx, Apache, or a built-in server in a framework) receives the HTTP request.
It determines how to handle the request based on the URL path, method, and other headers.

2. **Dynamic Content Generation**:

If the request requires dynamic content (e.g., a search query on Google), the server may:
Run server-side scripts (e.g., PHP, Python, Node.js).
Query databases (e.g., MySQL, PostgreSQL) for data.
Interact with other services or APIs.

3. **Static Content Retrieval**:

If the request is for static content (e.g., an image, CSS file), the server retrieves the file from disk or a cache.

4. **Response Creation**:

The server creates an HTTP response, which includes:
**Status Line**: E.g., HTTP/1.1 200 OK, indicating success.
**Response Headers**: E.g., Content-Type: text/html, Content-Length: ....
**Response Body**: The actual content (e.g., HTML, JSON, image data).

5. **Caching**:

The server may store the response in a cache (e.g., a CDN) for future requests.


###### Deep Dive [[How HTTP works over TCP ?]]
## Receiving the Response

The client (browser or curl) receives the response from the server and processes it according to its type.  

1. **Response Headers**:

The client reads the response headers to determine how to handle the response. For example:
Content-Type specifies the MIME type (e.g., text/html, application/json).
Set-Cookie might instruct the client to store cookies.
Cache-Control determines if the response can be cached.

2. **Response Body**:

The client reads the response body. The content type determines how the body is processed:
**HTML**: The browser parses and renders it.
**JSON/XML**: The browser or application might parse it into a data structure.
**Images/CSS/JS**: The browser fetches additional resources as needed.

3. **HTTPS (Decryption)**:

If the response is over HTTPS, the client decrypts the response using the session keys negotiated during the TLS handshake.

4. **Rendering (Browser Only)**:

The browser processes the HTML, CSS, and JavaScript:
**DOM Construction**: The HTML is parsed into a Document Object Model (DOM).
**CSSOM Construction**: CSS is parsed into a CSS Object Model (CSSOM).
**Rendering**: The DOM and CSSOM are combined to display the page.
**JavaScript Execution**: JavaScript is executed, potentially modifying the DOM or making further network requests (AJAX).

## Closing the Connection

After the response is received and processed, the connection between the client and server may be closed or kept alive for further requests.

1. **Connection Closure**:

**Four-Way Handshake**: If the connection is to be closed:
**FIN (Finish)**: The client sends a FIN packet to signal the end of the transmission.
**ACK**: The server acknowledges the FIN.
**FIN**: The server sends its own FIN.
**ACK**: The client acknowledges the server’s FIN, closing the connection.

2. **Keep-Alive**:

If the Connection: keep-alive header was sent, the TCP connection remains open for additional requests, reducing latency for future requests by avoiding the need to re-establish the connection.

3. **Persistent Connections**:

HTTP/1.1 connections are persistent by default unless explicitly closed, allowing multiple requests/responses over the same connection.