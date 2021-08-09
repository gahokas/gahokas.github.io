---
layout: default
---
# XRP Beginner Wallet FAQ

As a fairly new investor in XRP, I was horribly confused by wallets, how they work, and the best practices for using them. This is intended for retail investors of XRP with some general knowledge of cryptocurrencies who want to know more details of how XRP wallets work.

Have more questions? Let me know!

### What is an address?
An address refers to an account on the public XRP ledger. Addresses look like 
```
rGCkuB7PBr5tNy68tPEABEtcdno4hE6Y7f
```
You can think of an address as your account number on the public XRP ledger. Associated with your address on the public XRP ledger is how many XRP you own.

### What is a secret?
A secret is your “password” for your address, that allows you to create transactions (ie send XRP) from your address to another address. It is critical that you keep your secret safe and don’t lose it. Anyone who knows your secret can take all of your XRP. If you lose your secret, it is impossible to recover, and it is impossible to send XRP from your address. Your XRP will be stuck forever at your address, and you won’t be able to send or sell them.

Typically secrets look like:
```
sp6JS7f14BuwFY8Mw6bTtLKWauoUs
```
Secrets can also come in a few different formats, such as a series of letters or numbers.

### What is a wallet?
A wallet is some software (or hardware / software combination) that stores two pieces of data for you:
* Your address
* Your secret

Wallet software will allow you to create transactions on the public XRP leger. It does this by connecting to an XRP ledger server, and authorizing (signing) any XRP transaction from your address using your secret (actually, using your private / private key, which can be derived from your secret).

If you want to send XRP to a friend or business or an exchange, your wallet uses your address and secret to do so.

### What does a wallet hold?
A wallet will hold your address and your secret, that’s it. You may have multiple addresses (and multiple corresponding secrets) in your wallet if you like.

### A wallet doesn’t actually hold XRP?
Correct. XRP are **not** stored in your wallet. Your XRP are stored in the public XRP ledger. The public XRP ledger records how many XRP are associated with each address. This means if you know your someone’s address, you can see how many XRP they have! This is all public information.
