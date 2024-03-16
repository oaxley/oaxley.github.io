---
title: Message Cryptography with Python
tags: [security, python]
---
# Message Cryptography with Python

Let’s face it. We are no longer in the 90s where the Internet was a research project. Since then,
it has gone mainstream dragging with it a caravan of hackers (good and bad). We are no longer safe
in wonderland, and it’s more important than ever to encrypt your data. Be it personal data or the
one generated at the edge of the Internet by IOTs. As soon as you need to move it or store it
somewhere, trust no one and encrypt. Thankfully, Python has all that it takes to do it.

## **The Cryptography module**

As usual with Python, there is always a module that does what you want. And today is no exception.
The [Cryptography](https://cryptography.io/en/latest/) module is an open source project that aims
at providing best in class crypto to the masses.

First, start with the installation of the module:

```bash
pip install cryptography
```

Next, you need to know that the module has two parts. The first one, contains functions for doing
symmetric encryption or generating X509 certificates. The second part is called hazmat, an acronym
for “Hazardous Material”. This is the one that interests us because it covers asymmetric encryption,
key derivation, message authentication code and many more.

Hazmat is also a constant reminder that if you don’t do the cryptography right, sooner or later
your data can be stolen and in total jeopardy.

## **Private / Public keys**

Let’s dive into the asymmetric encryption by first generating our private and public RSA key.

As we mentioned in our post regarding
[OpenSSL](https://oaxley.github.io/security/2019/09/11/OpenSSL.html), you create
a private key secured with a password with:

```bash
openssl genpkey -algorithm rsa -pkeyopt rsa_keygen_bits:2048 | \
openssl pkcs8 -topk8 -v2 des3 -out private.pem
```

The corresponding public key is obtained with:

```bash
openssl pkey -in private.pem -pubout -out public.pem
```

Even if a password protects the private key, you still need to keep it in a safe storage.

## **Message Encryption**

To encrypt a message, you need to use your public key. Anybody who has access to your public key
can encrypt a message that only you can read.

So the first step is to create the Hazmat backend and read the public key.

```python
# get default backend
backend = backends.default_backend()

# read the public key
key_bytes = pathlib.Path("public.pem").read_bytes()
public_key = serialization.load_pem_public_key(key_bytes, backend)
```

To encrypt your message, some padding is necessary. Indeed, it needs to occur when your message
does not fit into a cipher block. Until recently, the preferred mode was the PKCS1 v1.5 but with
the advancements in technology, the OAEP (Optimal Asymmetric Encryption Padding) with SHA256
should now be used instead.

```python
# encrypt the message
cipher_bytes = public_key.encrypt(args.message.encode('utf-8'), padding.OAEP(
    mgf=padding.MGF1(algorithm=hashes.SHA256()),
    algorithm=hashes.SHA256(),
    label=None
) )
```

The encryption always returns Bytes. If you need to print the string, so the user can copy/paste
it somewhere, you should encode it with Base64:

```python
cipher = base64.b64encode(cipher_bytes)
```

## **Message Decryption**

The decryption of a message follows the exact same logic. We begin with the reading of the
private key without forgetting to provide the password as a Bytes object.

```python
# read the private key
key_bytes = pathlib.Path("private.pem").read_bytes()
private_key = serialization.load_pem_private_key(
    key_bytes, 
    password='supersecret'.encode('utf-8'), 
    backend=backend)
```

Then we need to apply the opposite Base64 operation (b64decode) to get back our cipher as a long
chain of Bytes.

```python
cipher_bytes = base64.b64decode(args.cipher)
```

To finish, we use the private key with the same OAEP padding to decode the cipher:

```python
message = private_key.decrypt(cipher_bytes, padding.OAEP(
    mgf=padding.MGF1(algorithm=hashes.SHA256()),
    algorithm=hashes.SHA256(),
    label=None
) )
```

## **Final Words**

The cryptography module has more functionalities than the two exposed today. You should
definitely have a look at least to know what’s possible.

Don’t hesitate to check the two resources below if you need more information on the cryptography:

- [Crypto 101](https://www.crypto101.io/), a free course on cryptography
- [Asymmetric Encryption](https://ssd.eff.org/en/module/deep-dive-end-end-encryption-how-do-public-key-encryption-systems-work)
  by Surveillance Self-Defense
