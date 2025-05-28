To get a certificate fingerprint like:

SHA256:qk0ns03ebk4A3DChUcbwEzRG4rUjyTlv10l7Lge88SE

you can use the ssh-keygen or openssl command, depending on the context:

⸻

✅ If you’re dealing with an SSH public key file (e.g., id_rsa.pub)

Use ssh-keygen:

ssh-keygen -lf ~/.ssh/id_rsa.pub -E sha256

Output:

2048 SHA256:qk0ns03ebk4A3DChUcbwEzRG4rUjyTlv10l7Lge88SE user@hostname (RSA)

	•	-l: Show fingerprint
	•	-f: File
	•	-E sha256: Use SHA256 hash format

⸻

✅ If you’re working with an X.509 SSL certificate (e.g., cert.pem)

Use openssl:

openssl x509 -in cert.pem -noout -fingerprint -sha256

Output:

SHA256 Fingerprint=6C:5A:BE:...:88:SE

If you want to match the base64-encoded SHA256 hash, use this:

openssl x509 -in cert.pem -noout -pubkey \
  | openssl pkey -pubin -outform DER \
  | openssl dgst -sha256 -binary \
  | openssl enc -base64

That will give you something like:

qk0ns03ebk4A3DChUcbwEzRG4rUjyTlv10l7Lge88SE=

(strip the = for GitHub or SSH format)

