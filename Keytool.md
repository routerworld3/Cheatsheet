# Keytool CheatSheet 

## Some history
This cheat sheet came into life when I started working on a tutorial of setting up one way tls and two way tls, which can be found here: [GitHub - Mutual TLS SSL](https://github.com/Hakky54/mutual-tls-ssl)

## Creation and importing
Generate a Java keystore and key pair
```
keytool -genkeypair -keyalg RSA -keysize 2048 -keystore keystore.jks -alias server -validity 3650
```

Generate a Java keystore and key pair and include Distinguished Name as one-liner and the Extensions
```
keytool -genkeypair -keyalg RSA -keysize 2048 -keystore keystore.jks -alias server -dname "CN=Hakan,OU=Amsterdam,O=Thunderberry,C=NL" -storepass secret -keypass secret -validity 3650 -ext KeyUsage=digitalSignature,dataEncipherment,keyEncipherment,keyAgreement -ext ExtendedKeyUsage=serverAuth,clientAuth -ext SubjectAlternativeName:c=DNS:localhost,DNS:IP:127.0.0.1
```

Generate a Java keystore and import a certificate
```
keytool -importcert -file server.crt -keystore truststore.jks -alias server
```

Generate a Root CA with signing capability
```
keytool -v -genkeypair -dname "CN=Root-CA,OU=Certificate Authority,O=Thunderberry,C=NL" -keystore root-ca.jks -storepass secret -keypass secret -keyalg RSA -keysize 2048 -alias root-ca -validity 3650 -ext KeyUsage=digitalSignature,keyCertSign -ext BasicConstraints=ca:true,PathLen:3
```

Generate a certificate signing request (CSR) for an existing Java keystore
```
keytool -certreq -keyalg rsa -keystore keystore.jks -alias server -file server.csr
```

Import a root or intermediate CA certificate to an existing Java keystore
```
keytool -import -trustcacerts -file root-ca.crt -alias my-newly-trusted-ca -keystore keystore.jks
```

Import the content of a keystore into another keystore
```
keytool -v -importkeystore -srckeystore source.p12 -srcstoretype PKCS12 -srcstorepass changeit -destkeystore target.p12 -deststoretype PKCS12 -deststorepass changeit
```

## Checking
Check a stand-alone certificate
```
keytool -v -printcert -file server.crt
```

Check a stand-alone certificate in PEM format
```
keytool -v -printcert -file server.crt -rfc
```

Check which certificates are in a Java keystore
```
keytool -v -list -keystore keystore.jks
```

Check a particular keystore entry using an alias
```
keytool -v -list -keystore keystore.jks -alias server
```

## Other commands
Delete a certificate from a Java keystore
```
keytool -delete -alias server -keystore keystore.jks
```

Change a Java keystore password
```
keytool -storepasswd -keystore keystore.jks
```

Signing a certificate with a certificate signing request (CSR)
```
keytool -v -gencert -infile server.csr -outfile server-signed.cer -keystore root-ca.jks -storepass secret -alias root-ca -validity 3650 -ext KeyUsage=digitalSignature,dataEncipherment,keyEncipherment,keyAgreement -ext ExtendedKeyUsage=serverAuth,clientAuth
```

Converting JKS to PKCS12
```
keytool -importkeystore -srckeystore keystore.jks -srcstoretype JKS -srcstorepass -destkeystore keystore.p12 -deststoretype PKCS12 password -deststorepass password
```

Converting PKCS12 to JKS
```
keytool -importkeystore -srckeystore keystore.p12 -srcstoretype PKCS12 -srcstorepass -destkeystore keystore.jks -deststoretype JKS password -deststorepass password
```

### Exporting
Export a certificate to a .crt file in a binary format
```
keytool -exportcert -keystore keystore.jks -alias server -file server.crt
```

Export a certificate to a .crt file in a pem format
```
keytool -exportcert -keystore keystore.jks -alias server -rfc -file server.crt
```

Export Java keystore to a .p12 file
```
keytool -importkeystore -srckeystore keystore.jks -destkeystore keystore.p12 -srcstoretype jks -deststoretype pkcs12
```
