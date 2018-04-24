---
published: false
title: 'Press F to Pay Respects: Your FTP Client is Dead'
---
## A Brief History of FTP

The uncreatively named FTP -- File Transfer Protocol -- is a protocol for file transfer dating back to 16 April 1971 (as [RFC 114](https://tools.ietf.org/html/rfc114)). Since then, it's had some updates, [RFC 765](https://tools.ietf.org/html/rfc765) and [959](https://tools.ietf.org/html/rfc959) moving the protocol on to TCP/IP, [RFC 1579](https://tools.ietf.org/html/rfc1579) and [2428](https://tools.ietf.org/html/rfc2428) adding some functionality, and [RFC 2228](https://tools.ietf.org/html/rfc2228) adding security extensions. Despite these updates, the core of FTP is much the same as it was in 1971, and many FTP clients written even today adhere closely to the minimum functionality required by the standard, in order to guarantee compatibility.

In general, an FTP connection flow looks something like this:
![Image credit http://www.tcpipguide.com/free/t_FTPControlConnectionEstablishmentUserAuthenticatio-2.htm](https://i.imgur.com/pg5KwSw.png)

## The Vulnerability

The security-astute minds out there may have already seen a problem in this protocol: **Nowhere in this standard flow is a maximum message size specified.** This introduces the possibility of a buffer overflow, since the default solution for many developers will go as follows:

- Have a local character array with some estimated maximum command length, say, 400 characters
- Upon receiving a message, copy it to the above array
- Handle the command

This would be fine, if there were some guarantee the message wouldn't exceed the estimated length. Unfortunately, since FTP doesn't specify a limit on the message size, it can be of any length. If the FTP application doesn't have a system to detect this and allocate the additional memory required, the message will likely by copied right over important data in the stack, like the return address pointer. This could be exploited to make the return address lead to some malicious code. In recent exploits (eg CVE-2018-7573), the tendency is to send a message consisting of a buffer full of "F" followed by a malicious return address, or else just an extremely large number of "F" characters to cause a segmentation fault in the application.

![Image credit Jonathan Ganz jonganz.com](https://i.imgur.com/2tiQT9l.png)

## The Attacks
In our research, we came across many instances of buffer overflows in FTP clients and servers, from the esoteric "FTPShell" client to the default client built in to Windows to the default server shipped with Microsoft's IIS server suite. The effects ranged from crashing the application (bad) to arbitrary code execution (really bad). Just to name a few:

- [CVE-2018-7573](https://www.cvedetails.com/cve/CVE-2018-7573/)
- [CVE-2017-6465](https://www.cvedetails.com/cve/CVE-2017-6465/)
- [CVE-2014-1215](https://www.cvedetails.com/cve/CVE-2014-1215/)
- [CVE-2010-3972](https://www.cvedetails.com/cve/CVE-2010-3972/)
- [CVE-2009-3364](https://www.cvedetails.com/cve/CVE-2009-3364/)
- [CVE-2009-1611](https://www.cvedetails.com/cve/CVE-2009-1611/)
- [CVE-2002-0608](https://www.cvedetails.com/cve/CVE-2002-0608/)
- [VU#276653](https://www.kb.cert.org/vuls/id/276653)

The prevalence of these vulnerabilities, the breadth of applications and vendors affected, and the frequency at which they recur all indicate that FTP client and server developers need to start taking steps to protect their applications. Of course, we can't expect every developer to independently spend their time coming up with a solution to protect their FTP applications from buffer overflows, and luckily a standardized (but optional) extension to FTP does exist to prevent these issues.

## Countermeasures
As mentioned, RFC 2228 does provide some extensions to help protect against this attack, namely the PBSZ ("protection buffer size") command. This works by allowing the client and server to negotiate a maximum packet/message size, providing a buffer length the client and server can use to prevent buffer overflow, but requires both the server and the client to implement its standard, which goes above and beyond what is required to make FTP work for its basic job of transferring files between well-meaning parties. Still, as demonstrated by the many vulnerability reports above, this attack vector will be discovered in an application sooner rather than later, and is well worth protecting against.