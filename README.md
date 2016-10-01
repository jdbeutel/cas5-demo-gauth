What is this project?
=====================

This extends the
[CAS Gradle Overlay](https://github.com/apereo/cas-gradle-overlay-template)
for a quick and dirty test/demo
of CAS 5 multi-factor authentication using Google Authenticator.
It's based on a CAS 5.0.0 RC3 snapshot,
just for me to try out Gradle, overlays, and the new CAS stuff.

It works for me.


Build
=====

* configure my Mac:

```bash
$ sudo mkdir /etc/cas
$ sudo chown jbeutel /etc/cas
$ sudo chgrp staff /etc/cas
$ mkdir /etc/cas/config
$ keytool -genkey -alias cas -keyalg RSA -validity 999 -keystore /etc/cas/thekeystore
Enter keystore password: changeit
Re-enter new password: changeit
What is your first and last name?
  [Unknown]:  localhost
What is the name of your organizational unit?
  [Unknown]:  Test
What is the name of your organization?
  [Unknown]:  Test
What is the name of your City or Locality?
  [Unknown]:  Test
What is the name of your State or Province?
  [Unknown]:  HI
What is the two-letter country code for this unit?
  [Unknown]:  US
Is CN=localhost, OU=Test, O=Test, L=Test, ST=HI, C=US correct?
  [no]:  yes

Enter key password for <cas>
	(RETURN if same as keystore password):  
	
$ ./gradlew clean build 
```

Test
====

* run the embedded Tomcat:  ```java -jar cas/build/libs/cas.war```
* navigate browser to this URL:
 ```https://localhost:8443/cas/login?service=https%3A%2F%2Fwww.apereo.org&authn_method=mfa-gauth```
* login w/
    * username: casuser
    * password: Mellon
* If this is the first time that casuser has logged in since Tomcat has restarted,
then a page like the following is shown:

    Your account is not registered.
    Use the below settings to register your device with CAS.
    
    (QR code)
    
    Secret key to register is TIRMDBIHKEA6INZF

    Scratch Codes:

    * 38243320
    * 53227858
    * 31646266
    * 27262512
    * 81434539
    
    [REGISTER]
* add QR code to my Android phone's Google Authenticator app via 'Scan a barcode'
* see "UHi (UHl:casuser)" added to my Google Authenticator
* click the [REGISTER] button
* see a page that requests just "Password:" with a [LOGIN] button
    * (fails to inform user that this should be the code from Google Authenticator)
    * (hides the input digits, like a password, even though they expire within 1 minute)
* enter the current 6-digit code from the added Google Authenticator
* if successful, browser navigates to www.apereo.org
    * (if unsuccessful, e.g., wrong or late code, then the password page just reloads, with no error message, although an exception is logged)



CAS Gradle Overlay
============================
Generic CAS gradle war overlay to exercise the latest versions of CAS. This overlay could be freely used as a starting template for local 
CAS gradle war overlays. 

## Versions

* CAS 5.0.0

## Requirements

* JDK 1.8+

## Configuration

The `etc` directory contains the configuration files that are copied to `/etc/cas/config`  automatically.

## Deployment

```bash
./gradlew[.bat] clean build && java -jar cas/build/libs/cas.war
```
