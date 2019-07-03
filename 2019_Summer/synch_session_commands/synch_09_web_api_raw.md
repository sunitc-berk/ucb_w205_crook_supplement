### Web API Raw Mode

You can use telnet to test unencrypted connections to a web API server.  He's how to connect to google.com:

```
telnet google.com 80
```

Inside the telnet session, you can issue a GET on / using HTTP version 1.0.  Version 1.0 hangs up after every request.

```
GET / HTTP/1.0
hit enter if no headers to be sent
```

Inside the telnet session, you can issue a GET on / using HTTP version 1.1.  It will hang on to the connection so you can make multiple requests.  If you hang up with a control-C it will also hang up.  If it sits a while without you entering a command, it will timeout.

```
GET / HTTP/1.1
hit enter if no headers to be sent
```

The response will be in this format:

* the first line will give you the version of HTTP, the return code, and the return code string. 200 is success.  400's are errors.

* the next line starts the headers in the format of key colon values.  note the cookies are set this way using headers.

* a blank line follows

* if it's HTTP/1.1, the next line has the number of bytes in the return payload.  We need this because we need to know when the message ends as it will be multiple TCP/IP packets.  if it's HTTP/1.0, this line is omitted

* the next line starts the return payload, typically HTML or encoded binary for images, videos, etc.

telnet only works for unencrypted unauthenticated http traffic.  If a website or web API server is running https, then the traffic will be encrypted and also must be authenticated. There is a utility in your virtual machine (available in all Linux versions that I know of) called openssl that we can use in place of telnet.

To connect to google.com using https:

```
openssl s_client -connect google.com:443
```

It will respond with the details of the authentication, including the certificate and the 3rd party validation chain for the security certificate

You can can enter commands as before with telnet.

One really good website to learn how to use a web API is **Where is the ISS at?**  This website tracks the location of the International Space Station.  It uses HTTPS and returns everything in JSON format.

https://wheretheiss.at/w/developer

you can connect like this:

```
openssl s_client -connect api.wheretheiss.at:443
```

you can try commands like these.  since they run multiple domains on the same ip address, we need to add a header with the specific domain:

```
GET /v1/satellites HTTP/1.1
Host: api.wheretheiss.at
hit enter if no more headers to be sent

GET /v1/satellites/25544/tles HTTP/1.1
Host: api.wheretheiss.at
hit enter if no more headers to be sent

GET /v1/satellites/25544/tles?format=text HTTP/1.1
Host: api.wheretheiss.at
hit enter if no more headers to be sent
```