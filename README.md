# Bastion Host And SQS VPC endpoint

**Bastion Host**

**What is a bastion host?**
A bastion host is a server used to manage access to an internal or private network from an external network - sometimes called a jump box or jump server. Because bastion hosts often sit on the Internet, they typically run a minimum amount of services in order to reduce their attack surface. They are also commonly used to proxy and log communications, such as SSH sessions.

![image](https://user-images.githubusercontent.com/74225291/164766272-fb2ec04b-399c-45b5-8e0f-9aa3ac5a61c6.png)



To connect to the bastion server it should have public access with a public IPv4 address, so let's launch an EC2 instance as Bastion Host.
And one more instance as a private instance in private subnet which is not hacing any public IP associated with it.

<img width="1466" alt="image" src="https://user-images.githubusercontent.com/74225291/164878382-c5b4c015-733c-4a4a-ae08-2760cfe11f22.png">

Public/Bastion Instance Details:

<img width="1467" alt="image" src="https://user-images.githubusercontent.com/74225291/164878410-eddfeb6c-3876-4156-91ed-a05f0cd75eb8.png">

Private Instance Details: There is no Public IP associated with it.

<img width="1452" alt="image" src="https://user-images.githubusercontent.com/74225291/164878430-124e692f-d62e-42e6-ac43-b636868364ff.png">

Now connect to bastion host from terminal and then from this connect to private instance.

      Tdaspute@INPNL000868 Downloads % ssh -i "eks-cluster.pem" ec2-user@ec2-52-14-84-9.us-east-2.compute.amazonaws.com
      The authenticity of host 'ec2-52-14-84-9.us-east-2.compute.amazonaws.com (52.14.84.9)' can't be established.
      ECDSA key fingerprint is SHA256:J1yjjgf1xpfCil2R9DcfU9AKJOUAO6Gh+RA7NqrzCZ0.
      Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
      Warning: Permanently added 'ec2-52-14-84-9.us-east-2.compute.amazonaws.com,52.14.84.9' (ECDSA) to the list of known hosts.

             __|  __|_  )
             _|  (     /   Amazon Linux 2 AMI
            ___|\___|___|

      https://aws.amazon.com/amazon-linux-2/
      -bash: warning: setlocale: LC_CTYPE: cannot change locale (UTF-8): No such file or directory
      [ec2-user@ip-10-0-0-133 ~]$ ping amazon.com
      PING amazon.com (205.251.242.103) 56(84) bytes of data.
      64 bytes from s3-console-us-standard.console.aws.amazon.com (205.251.242.103): icmp_seq=1 ttl=223 time=11.6 ms
      64 bytes from s3-console-us-standard.console.aws.amazon.com (205.251.242.103): icmp_seq=2 ttl=223 time=11.6 ms
      64 bytes from s3-console-us-standard.console.aws.amazon.com (205.251.242.103): icmp_seq=3 ttl=223 time=11.7 ms
      64 bytes from s3-console-us-standard.console.aws.amazon.com (205.251.242.103): icmp_seq=4 ttl=223 time=11.7 ms
      ^C
      --- amazon.com ping statistics ---
      4 packets transmitted, 4 received, 0% packet loss, time 3005ms
      rtt min/avg/max/mdev = 11.631/11.684/11.733/0.137 ms

Now copy pem file from local to bastio so that we can use it to connect to private one.

      Tdaspute@INPNL000868 Downloads % scp -i "eks-cluster.pem" eks-cluster.pem ec2-user@ec2-52-14-84-9.us-east-2.compute.amazonaws.com:~ 
      /etc/profile.d/lang.sh: line 19: warning: setlocale: LC_CTYPE: cannot change locale (UTF-8): No such file or directory
      eks-cluster.pem                                                                                                                    100% 1704     5.0KB/s   00:00    

Now connect to private instance from bastion :

      [ec2-user@ip-10-0-0-133 ~]$ ssh -i "eks-cluster.pem" ec2-user@10.0.3.71
      The authenticity of host '10.0.3.71 (10.0.3.71)' can't be established.
      ECDSA key fingerprint is SHA256:aFdPQkTJ21OMt3bntmINP3YZJqs9EeOK1B7mwEqWbbI.
      ECDSA key fingerprint is MD5:c3:89:8a:16:44:f9:21:02:f7:fa:32:cc:39:67:f9:a2.
      Are you sure you want to continue connecting (yes/no)? yes
      Warning: Permanently added '10.0.3.71' (ECDSA) to the list of known hosts.

             __|  __|_  )
             _|  (     /   Amazon Linux 2 AMI
            ___|\___|___|

      https://aws.amazon.com/amazon-linux-2/
      [ec2-user@ip-10-0-3-71 ~]$ 
      [ec2-user@ip-10-0-3-71 ~]$ 
      [ec2-user@ip-10-0-3-71 ~]$ 
      [ec2-user@ip-10-0-3-71 ~]$ ping amazon.com
      PING amazon.com (205.251.242.103) 56(84) bytes of data.
      ^C
      --- amazon.com ping statistics ---
      4 packets transmitted, 0 received, 100% packet loss, time 3067ms

      [ec2-user@ip-10-0-3-71 ~]$ 

**SQS VPC Endpoint**

Amazon announced that its fully managed message queuing service [Simple Queue Service (SQS)](https://aws.amazon.com/sqs/) supports Virtual Private Cloud (VPC) Endpoints using AWS PrivateLink. This enables customers to implement private access to SQS, and not have to use public IPs and traverse the public internet.

<img width="674" alt="image" src="https://user-images.githubusercontent.com/74225291/164879387-faac01f5-10a1-449c-a7b9-1c66309a56c7.png">


Create one simple SQS fifo Queue with defaul configuration.

<img width="1461" alt="image" src="https://user-images.githubusercontent.com/74225291/164879274-5fde2143-ae9d-4ffc-9a0d-fcf727bf1812.png">

<img width="1026" alt="image" src="https://user-images.githubusercontent.com/74225291/164879295-be9b00ce-d7ee-4b6f-8b68-cb617e28ab76.png">


Here we can see that there is no access to public internet from Private instance. Now we can create SQS VPC endpoint to access the sqs queue from private instance.

[ec2-user@ip-10-0-3-71 ~]$ curl -v https://sqs.us-east-2.amazonaws.com/358959533981/TestQueue.fifo
*   Trying 52.95.16.58:443...


* connect to 52.95.16.58 port 443 failed: Connection timed out
* Failed to connect to sqs.us-east-2.amazonaws.com port 443 after 130475 ms: Connection timed out
* Closing connection 0


Now we can add SQS vpc endpoint and recheck if we are able to access SQS queue or not.


<img width="1290" alt="image" src="https://user-images.githubusercontent.com/74225291/164766481-76f486cc-c81e-4af0-84c5-d09c4925b5c7.png">



 <img width="1286" alt="image" src="https://user-images.githubusercontent.com/74225291/164767559-155d5bf7-0148-414a-acfa-aeb071888336.png">


[ec2-user@ip-10-0-3-71 ~]$ curl https://sqs.us-east-2.amazonaws.com/358959533981/TestQueue.fifo
<UnknownOperationException/>

[ec2-user@ip-10-0-3-71 ~]$ curl -v https://sqs.us-east-2.amazonaws.com/358959533981/TestQueue.fifo
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
    

