.PHONY: all clean

all: ca.crt server.crt client.crt permissions

clean:
	rm -f *.crt *.key *.csr *.srl

permissions:
	chmod 644 client.key server.key ca.key

ca.crt: ca.key
	openssl req -x509 -new -nodes -key ca.key -sha256 -days 1024 -out ca.crt -subj "/CN=Test CA" -extensions v3_ca -config openssl.cnf

ca.key:
	openssl genrsa -out ca.key 2048

server.crt: server.csr ca.crt ca.key
	openssl x509 -req -in server.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out server.crt -days 500 -sha256 -extfile openssl.cnf -extensions v3_req

server.csr: server.key
	openssl req -new -key server.key -out server.csr -subj "/CN=service_name_test" -config openssl.cnf

server.key:
	openssl genrsa -out server.key 2048

client.crt: client.csr ca.crt ca.key
	openssl x509 -req -days 750 -in client.csr -sha256 -CA ca.crt -CAkey ca.key -CAcreateserial -out client.crt -extfile client.cnf -extensions client

client.csr: client.key
	openssl req -new -key client.key -out client.csr -sha256 -subj '/CN=Local Test Client'

client.key:
	openssl genrsa -out client.key 4096
