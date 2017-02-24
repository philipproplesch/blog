---
title: Transfer a .DE domain to Namecheap
date: 2016-02-16
tags:
  - domains
  - dns
---
I was recently *forced* to transfer a [.DE domain](https://en.wikipedia.org/wiki/.de) to another registrar. No big deal, right? Just get the authorization code and transfer the domain.

Unfortunately most of the major (probably non-German?) domain registrars (like [GoDaddy](https://ca.godaddy.com/) or [Namecheap](https://www.namecheap.com/)) do support new registrations for .DE domains, but **not** the transfer of an existing one which is obviously a major drawback for me in this case. 

[GoDaddy does currently not support the transfer of .DE domains](https://ca.godaddy.com/help/about-de-domain-names-5825). Namecheap does not mention anything in this matter on their website, so I asked them:

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr"><a href="https://twitter.com/philipproplesch">@philipproplesch</a> Our Transfer Concierge Team can submit a DE transfer manually for you. Please contact us in live chats or submit a ticket</p>&mdash; Namecheap.com (@Namecheap) <a href="https://twitter.com/Namecheap/status/697938636541706240">February 12, 2016</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

Luckily they provide a manual option, so I was able to chat with them and initiate the process. The conversation was very smooth and the domain showed up in my domain list on the next day.

In theory the process should end here, but \*sigh\* the Namecheap nameservers do not comply with the requirements of the [DENIC](https://www.denic.de/en/) / [NAST test tool](https://www.denic.de/en/service/tools/nast/). I guess I could have found out about this earlier, but I just did not expect this to be an issue at all. Lesson learned!

To bring this story to an end... The nameservers provided by [Rackspace Cloud DNS](https://www.rackspace.com/cloud/dns) are not compatible either, but luckily [Cloudflare](https://www.cloudflare.com/) (I am on the free plan) works great and provides a slick and easy-to-use user interface.
