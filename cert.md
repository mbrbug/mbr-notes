#### generate pkcs12
`openssl pkcs12 -export -out server.p12 -inkey server.key -in server.crt -certfile CACert.crt`  
#### Generate a new private key and Certificate Signing Request
`openssl req -out CSR.csr -new -newkey rsa:2048 -nodes -keyout privateKey.key`  
#### Generate a self-signed certificate (see How to Create and Install an Apache Self Signed Certificate for more info)
`openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout privateKey.key -out certificate.crt`  
#### Generate a certificate signing request (CSR) for an existing private key
`openssl req -out CSR.csr -key privateKey.key -new`  
#### Generate a certificate signing request based on an existing certificate
`openssl x509 -x509toreq -in certificate.crt -out CSR.csr -signkey privateKey.key`  
#### Remove a passphrase from a private key
`openssl rsa -in privateKey.pem -out newPrivateKey.pem`  


### Checking Using OpenSSL
If you need to check the information within a Certificate, CSR or Private Key, use these commands. You can also check CSRs and check certificates using our online tools.

#### Check a Certificate Signing Request (CSR)
`openssl req -text -noout -verify -in CSR.csr`  
#### Check a private key
`openssl rsa -in privateKey.key -check`  
#### Check a certificate
`openssl x509 -in certificate.crt -text -noout`  
#### Check a PKCS#12 file (.pfx or .p12)
`openssl pkcs12 -info -in keyStore.p12`  

### Converting Using OpenSSL
These commands allow you to convert certificates and keys to different formats to make them compatible with specific types of servers or software. For example, you can convert a normal PEM file that would work with Apache to a PFX (PKCS#12) file and use it with Tomcat or IIS. Use our SSL Converter to convert certificates without messing with OpenSSL.

#### Convert a DER file (.crt .cer .der) to PEM
`openssl x509 -inform der -in certificate.cer -out certificate.pem`  
#### Convert a PEM file to DER
`openssl x509 -outform der -in certificate.pem -out certificate.der`  
#### Convert a PKCS#12 file (.pfx .p12) containing a private key and certificates to PEM
`openssl pkcs12 -in keyStore.pfx -out keyStore.pem -nodes`  
You can add -nocerts to only output the private key or add - 
#### Convert a PEM certificate file and a private key to PKCS#12 (.pfx .p12)
`openssl pkcs12 -export -out certificate.pfx -inkey privateKey.key -in certificate.crt -certfile CACert.crt`  

### keytool | Java Keytool Commands for Creating and Importing
#### While we create a Java keystore, we will first create the .jks file that will initially only contain the private key using the keytool utility.
`keytool -genkey -keystore keystore.jks -alias ssl -validity 3650 -keysize 2048`  
 #### keytool import
 `keytool -import -keystore clientkeystore -file client.cer -alias client`  

 
These commands allow you to generate a new Java Keytool keystore file, create a CSR, and import certificates. Any root or intermediate certificates will need to be imported before importing the primary certificate for your domain.

Generate a Java keystore and key pair  
`keytool -genkey -alias mydomain -keyalg RSA -keystore keystore.jks  -keysize 2048`  
Generate a certificate signing request (CSR) for an existing Java keystore  
`keytool -certreq -alias mydomain -keystore keystore.jks -file mydomain.csr`  
Import a root or intermediate CA certificate to an existing Java keystore  
`keytool -import -trustcacerts -alias root -file Thawte.crt -keystore keystore.jks`  
Import a signed primary certificate to an existing Java keystore  
`keytool -import -trustcacerts -alias mydomain -file mydomain.crt -keystore keystore.jks`  
Generate a keystore and self-signed certificate (see How to Create a Self Signed Certificate using Java Keytoolfor more info)  
`keytool -genkey -keyalg RSA -alias selfsigned -keystore keystore.jks -storepass password -validity 360 -keysize 2048`  

#### Java Keytool Commands for Checking
If you need to check the information within a certificate, or Java keystore, use these commands.

Check a stand-alone certificate  
`keytool -printcert -v -file mydomain.crt`  
Check which certificates are in a Java keystore  
`keytool -list -v -keystore keystore.jks`  
Check a particular keystore entry using an alias  
`keytool -list -v -keystore keystore.jks -alias mydomain`  
#### Other Java Keytool Commands  
Delete a certificate from a Java Keytool keystore  
`keytool -delete -alias mydomain -keystore keystore.jks`  
Change a Java keystore password  
`keytool -storepasswd -new new_storepass -keystore keystore.jks`  
Export a certificate from a keystore  
`keytool -export -alias mydomain -file mydomain.crt -keystore keystore.jks`  
List Trusted CA Certs  
`keytool -list -v -keystore $JAVA_HOME/jre/lib/security/cacerts`  
Import New CA into Trusted Certs  
`keytool -import -trustcacerts -file /path/to/ca/ca.pem -alias CA_ALIAS -keystore $JAVA_HOME/jre/lib/security/cacerts`  