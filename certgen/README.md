## Create self-signed certificate

Create the Private Key. Press "enter" at the prompt for a passphrase:
openssl genpkey -algorithm RSA -aes128 -out private-ca.key -outform PEM -pkeyopt rsa_keygen_bits:2048

Create the CA. Press "enter" at the prompt for a passphrase:
openssl req -x509 -new -nodes -sha256 -days 825 -key private-ca.key -out self-signed-ca-cert.crt -config req.cnf

Create the Bitwarden key:
openssl genpkey -algorithm RSA -out bitwarden.key -outform PEM -pkeyopt rsa_keygen_bits:2048

Create the bitwarden certificate request file:
openssl req -new -key bitwarden.key -out bitwarden.csr

Create the bitwarden certificate, signed from the root CA:
openssl x509 -req -in bitwarden.csr -CA self-signed-ca-cert.crt -CAkey private-ca.key -CAcreateserial -out bitwarden.crt -days 365 -sha256 -extfile bitwarden.ext

Add this to the bottom of your /etc/hosts:
127.0.0.1       vault.example.com