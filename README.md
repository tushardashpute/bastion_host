# bastion_host


**What is a bastion host?**
A bastion host is a server used to manage access to an internal or private network from an external network - sometimes called a jump box or jump server. Because bastion hosts often sit on the Internet, they typically run a minimum amount of services in order to reduce their attack surface. They are also commonly used to proxy and log communications, such as SSH sessions.

![image](https://user-images.githubusercontent.com/74225291/164766272-fb2ec04b-399c-45b5-8e0f-9aa3ac5a61c6.png)



<img width="1290" alt="image" src="https://user-images.githubusercontent.com/74225291/164766481-76f486cc-c81e-4af0-84c5-d09c4925b5c7.png">



$ curl -v https://sqs.us-east-2.amazonaws.com/358959533981/TestQueue.fifo
*   Trying 52.95.16.58:443...


* connect to 52.95.16.58 port 443 failed: Connection timed out
* Failed to connect to sqs.us-east-2.amazonaws.com port 443 after 130475 ms: Connection timed out
* Closing connection 0



$ curl https://sqs.us-east-2.amazonaws.com/358959533981/TestQueue.fifo
<UnknownOperationException/>

$ curl -v https://sqs.us-east-2.amazonaws.com/358959533981/TestQueue.fifo
    *   Trying 10.0.0.229:443...

    * Connected to sqs.us-east-2.amazonaws.com (10.0.0.229) port 443 (#0)
    * ALPN, offering h2
    * ALPN, offering http/1.1
    * Cipher selection: ALL:!EXPORT:!EXPORT40:!EXPORT56:!aNULL:!LOW:!RC4:@STRENGTH
    * successfully set certificate verify locations:
    *  CAfile: /etc/pki/tls/certs/ca-bundle.crt
    *  CApath: none
    * TLSv1.2 (OUT), TLS header, Certificate Status (22):
    * TLSv1.2 (OUT), TLS handshake, Client hello (1):
    * TLSv1.2 (IN), TLS handshake, Server hello (2):
    * TLSv1.2 (IN), TLS handshake, Certificate (11):
    * TLSv1.2 (IN), TLS handshake, Server key exchange (12):
    * TLSv1.2 (IN), TLS handshake, Server finished (14):
    * TLSv1.2 (OUT), TLS handshake, Client key exchange (16):
    * TLSv1.2 (OUT), TLS change cipher, Change cipher spec (1):
    * TLSv1.2 (OUT), TLS handshake, Finished (20):
    * TLSv1.2 (IN), TLS change cipher, Change cipher spec (1):
    * TLSv1.2 (IN), TLS handshake, Finished (20):
    * SSL connection using TLSv1.2 / ECDHE-RSA-AES128-GCM-SHA256
    * ALPN, server did not agree to a protocol
    * Server certificate:
    *  subject: CN=sqs.us-east-2.amazonaws.com
    *  start date: Oct  5 00:00:00 2021 GMT
    *  expire date: Sep  9 23:59:59 2022 GMT
    *  subjectAltName: host "sqs.us-east-2.amazonaws.com" matched cert's "sqs.us-east-2.amazonaws.com"
    *  issuer: C=US; O=Amazon; OU=Server CA 1B; CN=Amazon
    *  SSL certificate verify ok.
    > GET /358959533981/TestQueue.fifo HTTP/1.1
    > Host: sqs.us-east-2.amazonaws.com
    > User-Agent: curl/7.79.1
    > Accept: */*
    > 
    * Mark bundle as not supporting multiuse
    < HTTP/1.1 404 Not Found
    < x-amzn-RequestId: 0d0009c6-c9f9-53c8-9ac6-531a2030d2ef
    < Date: Fri, 22 Apr 2022 17:36:09 GMT
    < Content-Length: 29
    < 
    <UnknownOperationException/>
    * Connection #0 to host sqs.us-east-2.amazonaws.com left intact
    
 <img width="1286" alt="image" src="https://user-images.githubusercontent.com/74225291/164767559-155d5bf7-0148-414a-acfa-aeb071888336.png">


