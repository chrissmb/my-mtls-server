# Root CA
openssl genrsa -des3 -out rootCA.key 2048
mycakey
openssl req -x509 -new -nodes -key rootCA.key -sha256 -days 1825 -out rootCA.pem

# Server
openssl genrsa -des3 -out server.key 2048
myserverkey
openssl req -new -sha256 -key server.key -out server.csr
myserverkey
openssl x509 -req -in server.csr -CA rootCA.pem -CAkey rootCA.key -CAcreateserial -out server.pem -days 365 -sha256

# Client
openssl genrsa -des3 -out client.key 2048
myclientkey
openssl req -new -sha256 -key client.key -out client.csr
myclientkey
openssl x509 -req -in client.csr -CA rootCA.pem -CAkey rootCA.key -CAcreateserial -out client.pem -days 365 -sha256

# Server P12
openssl pkcs12 -export -in server.pem -out keystore.p12 -name server -nodes -inkey server.key

# Trust store
keytool -import -file rootCA.pem -alias rootCA -keystore truststore.p12
