---
title: Battle - NodeJs vs Spring Webflux
draft: 
tags:
  - nodejs
  - java
  - spring-webflux
  - benchmark
---

Out of frustration and anger I decided to do compare these two framework

I don’t have anger issue but after having conversation with developers and reading on blogs etc. It seems everyone favours Node.js capabilities when it come to developing I/O applications and its irritating to hear that Java is not a good choice for it.

Here is what I have found from stackoverflow about what is I/O task and CPU intensive tasks are as the meaning of I/O heavily depends on prespective. I’m still not sure if these definition are correct. I’m open to disscuss and change them.

Please do tell if the code is not optimised or if I am testing in a wrong way.

## **Definitions**

I/O operation : Any process than does not consume CPU cycle. Calling a database, making http calls over network etc.

CPU operations: Any process or operation that consumer CPU cycle. Password Encryption, data manipulation, sorting, filtering, merge. Basically any data manipulation with data structure.

**System Config and versions**

OS: Ubuntu 22.04.1 LTS 64 Bit  
Memory: 32 GB  
Processor: 12th Gen Intel® Core™ i7–1270P  
Cores: 16

Node version : v19.3.0  
Spring boot version : 3.0.2-SNAPSHOT  
Java version : 18.0.2-ea

## **Phase I (String return)**

Creating a server on node using express by copying the code from official website that will return “Hello World!”, same step with spring-boot as well. Testing the performance using Apache ab with different concurrency and requests

Observations : Nodejs was consistent in ab results, on the other hand java took time to lauch thread at first run but once the thread connections are established it becomes consistent.

Running Process: Ran Nodejs express server using pm2 for utilizing all the cores, java doesn’t need any thing for it.

Below are the results

#### NODEJS   
```
❯ ab -n 1000000 -c 1000 "http://localhost:3000/"  
This is ApacheBench, Version 2.3 <$Revision: 1879490 $>  
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/  
Licensed to The Apache Software Foundation, http://www.apache.org/  
  
Benchmarking localhost (be patient)  
Completed 100000 requests  
Completed 200000 requests  
Completed 300000 requests  
Completed 400000 requests  
Completed 500000 requests  
Completed 600000 requests  
Completed 700000 requests  
Completed 800000 requests  
Completed 900000 requests  
Completed 1000000 requests  
Finished 1000000 requests  
  
  
Server Software:          
Server Hostname:        localhost  
Server Port:            3000  
  
Document Path:          /  
Document Length:        12 bytes  
  
Concurrency Level:      1000  
Time taken for tests:   46.510 seconds  
Complete requests:      1000000  
Failed requests:        0  
Total transferred:      211000000 bytes  
HTML transferred:       12000000 bytes  
Requests per second:    21500.80 [#/sec] (mean)  
Time per request:       46.510 [ms] (mean)  
Time per request:       0.047 [ms] (mean, across all concurrent requests)  
Transfer rate:          4430.34 [Kbytes/sec] received  
  
Connection Times (ms)  
              min  mean[+/-sd] median   max  
Connect:        0   11   7.4     12      38  
Processing:    13   35  14.8     31      92  
Waiting:        1   31  15.4     28      88  
Total:         25   46  11.1     43      92  
  
Percentage of the requests served within a certain time (ms)  
  50%     43  
  66%     48  
  75%     54  
  80%     58  
  90%     64  
  95%     67  
  98%     72  
  99%     77  
 100%     92 (longest request)  
```
  
  
## Java   
  
```
❯ ab -n 1000000 -c 1000 "http://localhost:8080/"  
This is ApacheBench, Version 2.3 <$Revision: 1879490 $>  
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/  
Licensed to The Apache Software Foundation, http://www.apache.org/  
  
Benchmarking localhost (be patient)  
Completed 100000 requests  
Completed 200000 requests  
Completed 300000 requests  
Completed 400000 requests  
Completed 500000 requests  
Completed 600000 requests  
Completed 700000 requests  
Completed 800000 requests  
Completed 900000 requests  
Completed 1000000 requests  
Finished 1000000 requests  
  
  
Server Software:          
Server Hostname:        localhost  
Server Port:            8080  
  
Document Path:          /  
Document Length:        12 bytes  
  
Concurrency Level:      1000  
Time taken for tests:   25.762 seconds  
Complete requests:      1000000  
Failed requests:        0  
Total transferred:      91000000 bytes  
HTML transferred:       12000000 bytes  
Requests per second:    38816.91 [#/sec] (mean)  
Time per request:       25.762 [ms] (mean)  
Time per request:       0.026 [ms] (mean, across all concurrent requests)  
Transfer rate:          3449.55 [Kbytes/sec] received  
  
Connection Times (ms)  
              min  mean[+/-sd] median   max  
Connect:        0   12   1.1     12      22  
Processing:     4   14   2.3     13      29  
Waiting:        1   10   2.1     10      26  
Total:         12   26   2.2     25      42  
  
Percentage of the requests served within a certain time (ms)  
  50%     25  
  66%     26  
  75%     27  
  80%     28  
  90%     29  
  95%     30  
  98%     31  
  99%     32  
 100%     42 (longest request)
```

Node.js Code:

```
const express = require('express')  
const app = express()  
const port = 3000  
  
app.get('/', (req, res) => {  
    
  res.send('Hello World!')  
})  
  
app.listen(port, () => {  
  console.log(`Example app listening on port ${port}`)  
})
```

Java Code:

```
package com.prakritidev.verma.javasever;  
  
import org.springframework.boot.SpringApplication;  
import org.springframework.boot.autoconfigure.SpringBootApplication;  
import org.springframework.web.bind.annotation.GetMapping;  
import org.springframework.web.bind.annotation.RequestMapping;  
import org.springframework.web.bind.annotation.RestController;  
  
@SpringBootApplication  
public class JavaseverApplication {  
  
 public static void main(String[] args) {  
  SpringApplication.run(JavaseverApplication.class, args);  
 }  
  
}  
  
@RestController  
@RequestMapping  
class Controller {   
   
 @GetMapping(path = "/")  
 public String helloWorld() {  
  return "Hello World!";  
 }  
  
}
```

## **Phase 2 : Simple Hashing implementation (CPU Task)**

Code reference: [https://stackoverflow.com/questions/7616461/generate-a-hash-from-string-in-javascript](https://stackoverflow.com/questions/7616461/generate-a-hash-from-string-in-javascript)

Below are the results:

  
## Phase 2 (Hashing Implementation)  
  
I was not expecting this result. will do some some more testing in this phase.   
  
### Java  
  
```
❯ ab -n 1000000 -c 1000 "http://localhost:8080/?input=duck"  
This is ApacheBench, Version 2.3 <$Revision: 1879490 $>  
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/  
Licensed to The Apache Software Foundation, http://www.apache.org/  
  
Benchmarking localhost (be patient)  
Completed 100000 requests  
Completed 200000 requests  
Completed 300000 requests  
Completed 400000 requests  
Completed 500000 requests  
Completed 600000 requests  
Completed 700000 requests  
Completed 800000 requests  
Completed 900000 requests  
Completed 1000000 requests  
Finished 1000000 requests  
  
  
Server Software:          
Server Hostname:        localhost  
Server Port:            8080  
  
Document Path:          /?input=duck  
Document Length:        7 bytes  
  
Concurrency Level:      1000  
Time taken for tests:   38.361 seconds  
Complete requests:      1000000  
Failed requests:        0  
Total transferred:      85000000 bytes  
HTML transferred:       7000000 bytes  
Requests per second:    26067.86 [#/sec] (mean)  
Time per request:       38.361 [ms] (mean)  
Time per request:       0.038 [ms] (mean, across all concurrent requests)  
Transfer rate:          2163.84 [Kbytes/sec] received  
  
Connection Times (ms)  
              min  mean[+/-sd] median   max  
Connect:        0   18   2.3     18      34  
Processing:     8   20   2.5     20      45  
Waiting:        1   14   2.5     14      38  
Total:         18   38   1.7     38      62  
  
Percentage of the requests served within a certain time (ms)  
  50%     38  
  66%     38  
  75%     39  
  80%     39  
  90%     40  
  95%     41  
  98%     43  
  99%     45  
 100%     62 (longest request)  
```  
  
### Nodejs  
  
```
 ab -n 1000000 -c 1000 "http://localhost:3000/?input=duck"  
This is ApacheBench, Version 2.3 <$Revision: 1879490 $>  
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/  
Licensed to The Apache Software Foundation, http://www.apache.org/  
  
Benchmarking localhost (be patient)  
Completed 100000 requests  
Completed 200000 requests  
Completed 300000 requests  
Completed 400000 requests  
Completed 500000 requests  
Completed 600000 requests  
Completed 700000 requests  
Completed 800000 requests  
Completed 900000 requests  
Completed 1000000 requests  
Finished 1000000 requests  
  
  
Server Software:          
Server Hostname:        localhost  
Server Port:            3000  
  
Document Path:          /?input=duck  
Document Length:        7 bytes  
  
Concurrency Level:      1000  
Time taken for tests:   105.177 seconds  
Complete requests:      1000000  
Failed requests:        0  
Total transferred:      205000000 bytes  
HTML transferred:       7000000 bytes  
Requests per second:    9507.81 [#/sec] (mean)  
Time per request:       105.177 [ms] (mean)  
Time per request:       0.105 [ms] (mean, across all concurrent requests)  
Transfer rate:          1903.42 [Kbytes/sec] received  
  
Connection Times (ms)  
              min  mean[+/-sd] median   max  
Connect:        0    2   4.7      0      36  
Processing:    12  103  21.3     99     238  
Waiting:        5  102  21.9     99     237  
Total:         26  105  19.8    100     238  
  
Percentage of the requests served within a certain time (ms)  
  50%    100  
  66%    103  
  75%    105  
  80%    107  
  90%    113  
  95%    139  
  98%    194  
  99%    209  
 100%    238 (longest request)
```
### Node.js Code:

```
const express = require('express')  
const app = express()  
const port = 3000  
  
  
app.get('/', (req, res) => {  
  const { input } = req.query  
  let hash = 0,  
  i, chr;  
  if (input.length === 0) return 0;  
  for (i = 0; i < input.length; i++) {  
    chr = input.charCodeAt(i);  
    hash = ((hash << 5) - hash) + chr;  
    hash |= 0; // Convert to 32bit integer  
  }  
    
    
  console.log(String(input), hash);  
  res.send(String(hash), 200);  
})  
  
app.listen(port, () => {  
  console.log(`Example app listening on port ${port}`)  
})
```

### Java Code :

```
package com.prakritidev.verma.javasever;  
  
import org.springframework.boot.SpringApplication;  
import org.springframework.boot.autoconfigure.SpringBootApplication;  
import org.springframework.web.bind.annotation.GetMapping;  
import org.springframework.web.bind.annotation.RequestMapping;  
import org.springframework.web.bind.annotation.RequestParam;  
import org.springframework.web.bind.annotation.RestController;  
  
@SpringBootApplication  
public class JavaseverApplication {  
  
 public static void main(String[] args) {  
  SpringApplication.run(JavaseverApplication.class, args);  
 }  
  
}  
  
@RestController  
@RequestMapping  
class Controller {  
  
 @GetMapping(path = "/")  
 public String helloWorld(@RequestParam String input) {  
  var hash = 0;   
  var i = 0;  
  char chr;  
    if (input.length() == 0) return "0";  
  for (i = 0; i < input.length(); i++) {  
   chr = input.charAt(i);  
   hash = ((hash << 5) - hash) + chr;  
   hash |= 0; // Convert to 32bit integer  
  }  
  
  return String.valueOf(hash);  
 }  
  
}
```
## Phase 3 : Redis set and fetch call (I/O)

… working on it

## Phase 4 : Mysql insert and fetch call (I/O)

… working on it

## Phase 5: Websocket implementation (I/O)

… working on it

## Phase 6: Backpressure implemenration

… working on it
