---
layout: default
---
# XRP Beginner Wallet FAQ

As a fairly new investor in XRP, I was horribly confused by wallets, how they work, and the best practices for using them. This is intended for retail investors of XRP with some general knowledge of cryptocurrencies who want to know more details of how XRP wallets work.

Have more questions? Let me know!

* [What is an address?](#what-is-an-address)
* [What is a secret?](#what-is-a-secret)
* [What is a wallet?](#what-is-a-wallet)
* [What does a wallet hold?](#what-does-a-wallet-hold)
* [A wallet doesn’t actually hold XRP?](#a-wallet-doesnt-actually-hold-xrp)
* [What is a wallet used for?](#what-is-a-wallet-used-for)
* [I don’t need to keep my address and secret in a wallet all the time?](#i-dont-need-to-keep-my-address-and-secret-in-a-wallet-all-the-time)
* [What kinds of wallets are there?](#what-kinds-of-wallets-are-there)
* [What is a custodial wallet?](#what-is-a-custodial-wallet)
* [How does an exchange keep track of whose XRP are whose with many customers using the same address?](#how-does-an-exchange-keep-track-of-whose-xrp-are-whose-with-many-customers-using-the-same-address)
* [When sending XRP to a custodial wallet on an exchange, is the destination tag mandatory?](#when-sending-xrp-to-a-custodial-wallet-on-an-exchange-is-the-destination-tag-mandatory)
* [Can I have multiple wallets with the same address?](#can-i-have-multiple-wallets-with-the-same-address)
* [Is it safe to keep my XRP in an exchange?](#is-it-safe-to-keep-my-xrp-in-an-exchange)
* [I’ve heard there is a 20 XRP reserve requirement. How does this work?](#ive-heard-there-is-a-20-xrp-reserve-requirement-how-does-this-work)
* [Will the 20 XRP minimum be reduced?](#will-the-20-xrp-minimum-be-reduced)
* [Can the 20 XRP reserve ever be recovered?](#id=can-the-20-xrp-reserve-ever-be-recovered)
* [Can I delete an address, or merge two addresses?](#can-i-delete-an-address-or-merge-two-addresses)
* [Why does my wallet show 20 XRP less than what I have?](#why-does-my-wallet-show-20-xrp-less-than-what-i-have)
* [If I have XRP in a wallet app on my phone and I lose my phone, is my XRP lost?](#if-i-have-xrp-in-a-wallet-app-on-my-phone-and-i-lose-my-phone-is-my-xrp-lost)
* [If I have XRP in a hardware wallet like a USB drive, and the drive fails, what happens?](#if-i-have-xrp-in-a-hardware-wallet-like-a-usb-drive-and-the-drive-fails-what-happens)
* [I lost/forgot my address, but I still have a copy of my secret. Can I recover my address?](#i-lostforgot-my-address-but-i-still-have-a-copy-of-my-secret-can-i-recover-my-address)
* [What are some popular XRP wallets?](#what-are-some-popular-xrp-wallets)
* [If I want to receive some XRP from someone (or from my account on an exchange), do I need a wallet?](#if-i-want-to-receive-some-xrp-from-someone-or-from-my-account-on-an-exchange-do-i-need-a-wallet)


## What is an address?
An address refers to an account on the public XRP ledger. Addresses look like 
```
rGCkuB7PBr5tNy68tPEABEtcdno4hE6Y7f
```
You can think of an address as your account number on the public XRP ledger. Associated with your address on the public XRP ledger is how many XRP you own.

## What is a secret?
A secret is your “password” for your address, that allows you to create transactions (ie send XRP) from your address to another address. It is critical that you keep your secret safe and don’t lose it. Anyone who knows your secret can take all of your XRP. If you lose your secret, it is impossible to recover, and it is impossible to send XRP from your address. Your XRP will be stuck forever at your address, and you won’t be able to send or sell them.

Typically secrets look like:
```
sp6JS7f14BuwFY8Mw6bTtLKWauoUs
```
Secrets can also come in a few different formats, such as a series of letters or numbers.

## What is a wallet?
A wallet is some software (or hardware / software combination) that stores two pieces of data for you:
* Your address
* Your secret

Wallet software will allow you to create transactions on the public XRP leger. It does this by connecting to an XRP ledger server, and authorizing (signing) any XRP transaction from your address using your secret (actually, using your private / private key, which can be derived from your secret).

If you want to send XRP to a friend or business or an exchange, your wallet uses your address and secret to do so.

## What does a wallet hold?
A wallet will hold your address and your secret, that’s it. You may have multiple addresses (and multiple corresponding secrets) in your wallet if you like.

## A wallet doesn’t actually hold XRP?
Correct. XRP are **not** stored in your wallet. Your XRP are stored in the public XRP ledger. The public XRP ledger records how many XRP are associated with each address. This means if you know your someone’s address, you can see how many XRP they have! This is all public information.

## What is a wallet used for?
A wallet is used as a convenient place to 1) hold a copy of your address and secret, and 2) interact with the public XRP ledger to send XRP or check your balance. You don’t need to keep your address and secret in a wallet, but it makes it easier.

## I don’t need to keep my address and secret in a wallet all the time?
Correct. All you need to do is store your address and secret somewhere safe. You can keep them in a text file on your computer, or you can print them out on a piece of paper and keep that safe if you like. If you need to check your balance or send some XRP, you can add the address and secret to a wallet, perform the transaction, and then remove the address and secret from the wallet.

## What kinds of wallets are there?
Wallets tend to be either pure software (like Xumm) on your computer or phone, or hardware / software combination (USB drive + software like the Ledger Nano S).

## What is a custodial wallet?
A custodial wallet is a shared wallet on a cryptocurrency exchange.

When you buy XRP on an exchange, the exchange doesn’t give each customer their own address on the public XRP ledger because of the reserve requirements and transaction costs (see below). When you buy or sell (or simply hold) XRP in an account on an exchange, your XRP are stored at a shared ledger address that is shared with many other customers at the exchange.

It is up to the exchange to keep track of how many XRP each customer owns. This opens many security concerns, and if you hold your XRP in an exchange, you must trust the exchange’s security mechanisms.

## How does an exchange keep track of whose XRP are whose with many customers using the same address? 
They use a destination tag, which works as an account number at the exchange.

If you were to send XRP to your account at the exchange, you would send it to the exchange address (shared with many people) and include a destination tag (specific to you that the exchange assigns) so the exchange knows that the XRP received is for you. 

It is up to the exchange to keep track of how many XRP each customer owns.

## When sending XRP to a custodial wallet on an exchange, is the destination tag mandatory?
Generally yes. Let’s say you have an XRP wallet with some XRP that you wish to sell. You send your XRP from your wallet address to your exchange account in order to sell them. Remember, your address at the exchange is shared between many other customers. If you were to send XRP to the exchange’s address without including your assigned destination tag, how would the exchange know which customer the XRP is for? They wouldn’t.

Some exchanges will actually reject transfers to their addresses that don’t include a destination tag, to protect customers who forget to include it.

Now, depending on how friendly your exchange is, if you were able to send XRP to an exchange address without specifying the destination tag, they may be willing to look at their transaction logs, see the specific amount you sent and time of your transaction, and assign the XRP to you. But, this depends on if your exchange customer support is willing to do this for you.

## Can I have multiple wallets with the same address?
Absolutely. Remember, all addresses and associated XRP are all on the public XRP ledger. A wallet simply allows access to an address. So there is nothing stopping you from adding your address (and secret) to multiple wallets. If one wallet were to send some XRP from that address, all the other wallets would be immediately updated when they connect to the public XRP ledger to check the balance of the address.

## Is it safe to keep my XRP in an exchange?
When you keep your XRP in an exchange, you are trusting the exchange with your XRP, just like trusting a bank with your money. The exchange is trusted in keeping safe all the secrets for their ledger addresses they use to store their customers’ XRP. If those secrets are compromised, their customers could lose their XRP. If the exchange is hacked or compromised (or even goes out of business), you could lose all your XRP.


## I’ve heard there is a 20 XRP reserve requirement. How does this work?
Built into the XRP Ledger are some rules that help combat spam and malicious activity. One of these is the reserve requirement.  In order to activate a new address on the XRP ledger, it must have at least 20 XRP associated with it. These 20 XRP are “locked” and cannot be transferred out or sold. You still own them, but they are used to keep your ledger address active.

The reserve requirement encourages XRP holders to be conservative with their use of addresses, and it slows the growth of the size of the ledger, since it costs real money to activate a new address.

For retail investors then, you would generally want to limit the number of addresses you have.

## Will the 20 XRP minimum be reduced?
Possibly at some point in the future, it could be reduced. It was reduced already from 50 XRP to 20 XRP in 2013. See [changing the reserve requirements](https://xrpl.org/reserves.html#changing-the-reserve-requirements){:target="_blank"}

## Can the 20 XRP reserve ever be recovered?
There are only two ways: 1) the 20 XRP reserve requirement is reduced, allowing access to some of the reserve, or 2) you delete an address, and send the XRP associated with that address to another address.

## Can I delete an address, or merge two addresses?
You can delete an address, and send the associated XRP to another address, effectively “merging” the two addresses. When you delete an address, there is a cost of 5 XRP. These 5 XRP is not a commission or fee charged by any exchange or wallet software. It is a fee built into the public XRP ledger. The 5 XRP are destroyed and can’t be recovered, again as an anti-spam mechanism to encourage purposeful use.

## Why does my wallet show 20 XRP less than what I have?
It’s likely your wallet software showing you how much XRP you actually have available. The software is subtracting off the 20 XRP reserve requirement. Your true balance is 20 XRP more.

## If I have XRP in a wallet app on my phone and I lose my phone, is my XRP lost?
Not necessarily. This is dependent on the security of your phone, the security of your XRP wallet app, and whether you kept a copy of your address and secret.

On the phone, the person finding your phone would first need to unlock it. Wallet apps (like XUMM) will have a security passcode that you create when you install the app, so the person would need to know that too in order to transfer XRP out of your address.

Assuming you kept a copy of your address and secret, you can simply add the address and secret to a new wallet on your new phone. At this point you could then decide if you wanted to create a new address, delete your old address, and transfer all your XRP to your new address. This would cost you 5 XRP.

## If I have XRP in a hardware wallet like a USB drive, and the drive fails, what happens?
As long as you have a copy of your address and secret, you’re fine. Destroy the USB drive, get another, and add your address and secret to it.

## I lost/forgot my address, but I still have a copy of my secret. Can I recover my address?
Yes. The XRP Ledger allows you to generate (or re-generate) a public / private key from a secret. The public key can then be used to generate the corresponding address.

Some software wallets will do this for you automatically. When you add your secret to XUMM for example, it will automatically calculate your address;  you just need to confirm it.

## What are some popular XRP wallets?
A list of some wallets are on the [XRPL site](https://xrpl.org/xrp-overview.html#wallets){:target="_blank"}

XUMM seems to be the most software popular wallet, and the Ledger Nano S seems to be a popular hardware wallet. I’ve used XUMM and it works well.

## If I want to receive some XRP from someone (or from my account on an exchange), do I need a wallet?
No. Once you have an address / secret pair (wallet software can generate one for you), your address on the public XRP ledger can receive XRP from anyone without any action on your part. Once the XRP has been transferred to your address, your wallet software would show your updated balance once it connects to an XRP ledger server.

