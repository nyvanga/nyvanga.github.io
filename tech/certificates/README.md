# Certificates

All the stuff you do not want to rediscover!!!

## Self-signed CA certificate

Become your own certificate issuer.

Throughout this guide some environment variables will be used:
```
HOSTNAME=some-host
LOCALDOMAIN=some-domain
ADDRESS=127.0.0.1
```

### CA Key

First you need a key for the CA-cert (The master key!):
```
openssl genrsa -out ca-cert.key 4096
```

### CA Certificate

Then generate the CA-cert itself:
```
openssl req \
	-new -key ca-cert.key \
	-x509 -days 3650 \
	-out ca-cert.crt \
	-subj "/CN=Evil overlords of the Internet"
```
**NOTE!** The value specified by `/CN=<name>` becomes the value shown in browser as `Verified by: <name>` so choose it wisely.

## Self-signed certificate

Generate a certificate and sign it with your CA-certificate.

### Key

First generate a key for the certificate:
```
openssl genrsa -out some-host.key 4096
```

### CSR (Certificate Signing Request)

Then to create a certificate with the least problems, it needs to match:
* hostname
* hostname.localdomain
* ip-address

To do that, create a csr conf file
```
cat << EOF > some-host.csr.conf
[ req ]
default_bits = 4096
prompt = no
default_md = sha256
req_extensions = req_ext
distinguished_name = dn

[ dn ]
CN = ${ADDRESS}

[ req_ext ]
subjectAltName = @alt_names

[ alt_names ]
DNS.1 = ${HOSTNAME}
DNS.2 = ${HOSTNAME}.${LOCALDOMAIN}
IP.1 = ${ADDRESS}

[ v3_ext ]
authorityKeyIdentifier=keyid,issuer:always
basicConstraints=CA:FALSE
keyUsage=keyEncipherment,dataEncipherment
extendedKeyUsage=serverAuth,clientAuth
subjectAltName=@alt_names
EOF
```

With the csr conf file, create the csr file:
```
openssl req -new -key some-host.key -out some-host.csr -config some-host.csr.conf
```

## Certificate
Finally the certificate can be created and signed:
```
openssl x509 -req -in some-host.csr \
	-CA ca-cert.crt -CAkey ca-cert.key -CAcreateserial \
	-out some-host.crt \
	-days 3650 -extensions v3_ext \
	-extfile some-host.csr.conf
```

## Build a JSSE keystore with key and certificate for TLS (Jetty and others)

The main tool for this is `keytool` which is distributed with JDK's and JRE's.

Key tool cannot import both a certificate and the private key, at once.

### PKCS12 Keystore
So first a temporary `pkcs12` keystore has to be created:
```
openssl \
	pkcs12 \
	-inkey some-host.key \
	-in some-host.crt \
	-export \
	-out some-host.pkcs12 \
	-name ${HOSTNAME}.${LOCALDOMAIN} \
	-passout pass:TemporaryPassword
```
**NOTE!** The `-name` option is going to be the `alias` in the JKE keystore.

### JKS Keystore
Then the `pkcs12` can be imported into a JKS keystore.
```
keytool \
	-importkeystore \
	-srckeystore some-host.pkcs12 \
	-srcstoretype PKCS12 \
	-srcstorepass 'TemporaryPassword' \
	-destkeystore some-keystore.jks \
	-deststoretype PKCS12 \
	-destkeypass 'changeit' \
	-deststorepass 'changeit'
```

### Show contents of JKS keystore
```
keytool -list -v -storepass changeit -keystore some-keystore.jks
```