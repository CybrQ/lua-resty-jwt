## For old test certificates

Those certs have been retrieved from http://fm4dd.com/openssl/certexamples.htm

- Root CA cert: http://fm4dd.com/openssl/source/PEM/certs/frank4dd-cacert.pem
- 2048b RSA cert: http://fm4dd.com/openssl/source/PEM/certs/2048b-rsa-example-cert.pem
- 2048 RSA keypair: http://fm4dd.com/openssl/source/PEM/keys/2048b-rsa-example-keypair.pem

---

this was my attempt earlier to make a new set of certs/keys

## Create root CA key

openssl genrsa -des3 -out rootCA.key 4096

## Create root CA cert

openssl req -x509 -new -nodes -key rootCA.key -sha256 -days 1024 -out rootCA.crt

## Pass phrase when prompted

`test`

## Instructions

1. Create PEM key

susie114:~> time openssl genrsa 2048 > source/PEM/keys/2048b-rsa-example-keypair.pem
Generating RSA private key, 2048 bit long modulus
....................................++++++
..++++++
e is 65537 (0x10001)

real    0m0.068s
user    0m0.064s
sys     0m0.002s

2. Convert PEM to DER key

susie114:~> openssl rsa -inform PEM -in  source/PEM/keys/2048b-rsa-example-keypair.pem -outform DER -out source/DER/keys/2048b-rsa-example-keypair.der

3. Create PEM request

susie114:~> openssl req -new -key source/PEM/keys/2048b-rsa-example-keypair.pem -out source/PEM/requests/2048b-rsa-example-request.pem
You are about to be asked to enter information that will be incorporated
into your certificate request.
susie114:/srv/www/std-root/frank4dd.com/howto/openssl # openssl req -new -key source/PEM/keys/2048b-rsa-example-keypair.pem -out 2048b-rsa-example-request.pem
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [US]:US
State or Province Name (full name) [Some-State]:New York
Locality Name (eg, city) []:
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Testing, Inc
Organizational Unit Name (eg, section) []:
Common Name (eg, YOUR name) []:www.example.com
Email Address []:

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:

4. Convert PEM to DER request

susie114:~> openssl req -inform PEM -in source/PEM/requests/2048b-rsa-example-request.pem -outform DER -out source/DER/requests/2048b-rsa-example-request.der

5. Create PEM certificate

susie114:~> openssl x509 -req -in source/PEM/requests/2048b-rsa-example-request.pem -days 1825 -CA source/PEM/certs/frank4dd-cacert.pem -CAkey /srv/app/webCA/private/cakey.pem -set_serial 0x0dfe > 2048b-rsa-example-cert.pem
Signature ok
subject=/C=US/ST=New York/O=Testing, Inc/CN=www.example.com
Getting CA Private Key
Enter pass phrase for <cert>:

6. Convert PEM to DER certificate

susie114:~> openssl x509 -inform PEM -in source/PEM/certs/2048b-rsa-example-cert.pem -outform DER -out source/DER/certs/2048b-rsa-example-cert.der

7. Create PKCS12 certificate filebundle (Password: test)

root@susie114:~> openssl pkcs12 -in source/PEM/certs/2048b-rsa-example-cert.pem -inkey source/PEM/keys/2048b-rsa-example-keypair.pem -export -out source/P12/2048b-rsa-example.p12
