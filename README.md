# tor-misconception
An attempt to document commonly believed misconceptions about Tor.

Content will appear here once I have gained sufficient vitriol...

# VPN
## A VPN is more secure because you can't trust Exits
### What does a VPN see?
* Who you are.
* Who you're talking to.
* What you and the other party are discussing.

### What does an Exit see?
* Who _someone_ is talking to.
* What _someone_ and the other party are discussing.

#### So what does this mean?
Well, the Exit can do everything the VPN can do but it can't do it to you consistently or in a targeted manner. You are less targetable than Tor. Your VPN can target you, and do so consistently.

### But what about...?
- The VPN can be trusted, I know because _magic_! So it won't tamper with my connection.

   The VPN isn't the only party in the connection. Your connection to the destination goes through a series of other connecting ISPs routers. Even if you trust your VPN and it turned out you were right, any single router on that connection can tamper with the connection, just like the Exit. Even if the router can't _directly_ tamper with the connection, visibility of the connection is enough for them to perform a "Man-on-the-Side" attack, by sending a response to your TCP connection opening before the other side has had a chance to response, they can inject content into the stream with Mallory outside of the connection and a complicit Eve in the connection.

- I connect to my VPN over Tor and pay for it anonymously.

   This has serious technical drawbacks that I'll cover later on, but now you have a VPN that doesn't know who you are but you better hope it stays that way forever and you never even make 1 mistake. If you slip up and send identifying information in plaintext over the VPN, you just contaminated: _everything_ you did on that VPN account with the other identity and _everything_ you did with that payment method. You really better have some good redundancies in place.
