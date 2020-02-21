ROOT_CA = ca

certs: root server client

root:
	@cfssl gencert -initca config/$(ROOT_CA)-csr.json | cfssljson -bare $(ROOT_CA)
	@rm $(ROOT_CA).csr

server:
	@cfssl gencert \
	-ca=$(ROOT_CA).pem \
	-ca-key=$(ROOT_CA)-key.pem \
	-config=config/$(ROOT_CA)-config.json \
	-profile=server \
	config/server-csr.json | cfssljson -bare server
	@rm server.csr

client:
	@cfssl gencert \
	-ca=$(ROOT_CA).pem \
	-ca-key=$(ROOT_CA)-key.pem \
	-config=config/$(ROOT_CA)-config.json \
	-profile=client \
	config/client-csr.json | cfssljson -bare client
	@rm client.csr

clean:
	rm -f *.csr *.pem
