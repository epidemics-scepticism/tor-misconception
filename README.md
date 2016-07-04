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

## Indinstinguishability
### TCP and IP stack metadata
VPNs function fundamentally differently from Tor. A VPN works by encapsulating a full IP packet. This means that when the packet is reconstructed at the other end of the VPNs connection and sent out, the IP and TCP metadata is indicative of your local system.

By comparison, Tor encapsulates only the stream data of a TCP connection. This means that when it is reconstructed into a TCP stream at the other side, the IP and TCP metadata comes from the exit, and is indistinguishable from all others users using that exit.

This metadata does not constitute a unique fingerprint but it can be used to distinguish between operating systems and even version of operating systems. For an example of a practical application of this, see [lcamtuf's `p0f`](http://lcamtuf.coredump.cx/p0f3/).

# ExitNodes, ExcludeNodes and GeoIP
## GeoIP is bullshit
### How does it know?
It doesn't.

### Why does it think it knows?
GeoIP works by looking at who the IP belongs to, often this can be as simple as just parsing the address on a whois record. Whois records are not authoritative _(and for the paranoid, whois is not resistant to adversarial attacks)_. If you look at the Tor relay [BC630CBBB518BE7E9F4E09712AB0269E9DC7D626](https://atlas.torproject.org/#details/BC630CBBB518BE7E9F4E09712AB0269E9DC7D626) you'll notice that according to GeoIP it's in Liberia. It's whois record states `country:        LR`. This is where the evidence for it's being in Liberia ends and infact you can show evidence that it is _not_ in Liberia with ease and trivially discover it's likely location.

### IPs aren't tied to a location.
Another thing you will notice is that often databases take a while to be updated. Blocks of IP addresses can change hands between companies. One day they're being used in France, the next they're in the Mexico. I ran a relay that for a period of months was incorrectly reported as being on the wrong side of the planet.

### Reduced circuit paths.
When you try to exclude certain nodes from your circuits you reduce the number of possible pathways that you can take through the Tor network. This makes your circuits different, and more limited, from the set of all other Tor users. There are cases where an adversary could fingerprint a user based on any policies they have set. For example, if an adversary could inject content into a page that caused you to load resources from a large range of first party domains on Tor Browser, you would use a different circuit for every resource. An adversary who could see those resource fetches could then see where you do and do not exit from. At any given time there is a certain statistical probability that you would exit from a node in the US, a probability from Germany, a probability for France and so on. They could see if your distribution of chosen exits matched this statistical likelyhood and see which were missing. This fingerprint would persist and would put you into a smaller set of users than the general set of Tor Browser users.

### Circuits pass through intermediaries.
If you wanted to exclude a certain country from your choice for some perceived good reason, this will not work. Circuits will happily be built on networks that traverse resources within these countries. The paths that circuits take is entirely outside of your control.

### I suspect this relay is doing bad things.
If you think a relay is being bad, instead of adding it to your ExcludeNodes you should report it by emailing bad-relays at lists.torproject.org

If possible provide the relay's fingerprint and what bad thing it is doing, steps to reproduce the bad behaviour are also very helpful.

### So what does this mean?
Reducing the location that you will or will not exit from harms your anonymity. There are cases where an adversary can use this as a method to fingerprint you and reduce your anonymity set.

Attempting to avoid countries because you don't trust the government in charge of them is folly. Believing this helps against any kind of well funded or technically capable attacker is also folly. It is only harming you.
