---
layout: post
title: Troubleshooting IIS Client Certificate issues
excerpt_separator: <!--more-->
---
IIS has the option to require client certificates, but setting this up requires all settings to be set correctly.

### The question
The customer question was that during a security audit, the server returned no root certificates, and not a list of trusted certificates. Error: No client certificate CA names sent
<!--more-->
The command used: OpenSSL s_client -connect site.customer.com:443

A certificate trust list needs to be enabled

### Investigating
The behavior to send the trusted issuer list by default is off in Server 2012 and newer. The default value of the SendTrustedIssuerList registry key is now 0 (off by default), instead of 1.

http://forums.iis.net/t/1223412.aspx?IIS+8+5+offers+all+client+certificates

HKLM\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL\SendTrustedIssuerList = 1

https://technet.microsoft.com/en-au/library/dn786429.aspx

THere is a lot of false info regarding this on the internet, about "client certificate mapping" and "clientauthtrustmode"

NETSH HTTP SHOW SSLCERT

Should display a "Ctl store name" when configured correctly.

### The solution
Enable "Negotiate client certificates" this will sent the list directly upon connecting

The real solution
The security requirement should be changed as well, displaying a list of trusted root certificates will help attackers, by adverstising what a valid certificate is.

A better security check would be that the security auditor test the connection with several valid certificates from different root CA's to validate that only the correct CA's are trusted.

Only checking what a server advertises is not a good idea.