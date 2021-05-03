---
layout: draft
title: Metadata Analysis of flatmap dependency supply chain attack
subtitle: Investigating future detection mechanisms for open source project repositories
#cover-img: /assets/img/solarwindsceocv.png
thumbnail-img: /assets/img/post2/
share-img: /assets/img/post2/
tags: [supplychain, sdlc]
---

There's been hundreds of software dependency supply chain attacks exploiting a range of vectors in the past. [The July  2020 paper by Marc Ohm et al ](https://link.springer.com/chapter/10.1007%2F978-3-030-52683-2_2) describes that on average **a malicious package is available for 209 days.** `(𝑚𝑖𝑛=−1,𝑚𝑎𝑥=1,216,𝜎=258,𝑥̃ =67)`

 In this post, I want to document a really interesting detection vector that I am attempting to operationalize using software health metrics from readily available metadata in an attempt to begin a compendium of metadata based detection aids for some attack vectors related to this problem.

This method is not really applicable or intended for typosquatted packages (although a lack of metadata is a signal in and of itself), but more for situations where a threat actor gains publishing permissions or control over an existing repository.

 #### from an attackers perspective, modifying a known good package's source code has several stages:

**1) Identification of a target**
Not withstanding opportunistic account acquisition, projects with low maintainer counts, long periods with no commits, longstanding issues make for good targets.
Tooling like CHAOSS, and OSSF Metrics are useful for both attackers and defenders in selecting targets.
**2) Determination of malicious code entry vector**
Social Engineering (working to obtain trusted status on the repositories, or PR's for issues), Obtaining publishing rights, Repo Takeovers are the most common entry vectors.
**3) Commit(s) of malicious content**
Once code is committed to the repository, the attacker is exposed to any gating mechanisms. Lengthy peer review periods or branch protections put the attack at risk.



![bad luck](/assets/img/solarwindsceocv.png){: .mx-auto.d-block :}

#### How else could one explain such misfortune?
I reference:
['SolarWinds'](https://en.wikipedia.org/wiki/2020_United_States_federal_government_data_breach)
['APT Actors Leverage Pulse Secure 0day'](https://www.fireeye.com/blog/threat-research/2021/04/suspected-apt-actors-leverage-bypass-techniques-pulse-secure-zero-day.html)

I've learned that this kind of misfortune caused by 'malice hexes'.

With a photo printer and simple household ingredients one can create powerful curses leveraging what insiders call 'sympathetic magic' to cause significant disruption to people's lives.

Objects like nails can be used to ''pierce'' the defenses of networks and shards of glass or rusted objects can be used for malice and pain. Black string is used to bind the spell to the individual.

![Hex Bags](/assets/img/hexbag.jpeg){: .mx-auto.d-block :}


By simply bundling the items in cloth and burning a candle, you too can start the process of applying, or remediating a curse. (Remediation in the following section). Simple incantations can be repeated to begin the process.

It should come as no surprise to even the most ardent skeptic that there is significant power in magic and also significant power within the psychic gestalt. This was probably best represented to those outside this domain of expertise in the movie 'Mean Girls (2004)' . ( IMDB 7.0/10? Are you serious? )

![Burn Book](/assets/img/burnbook.jpeg){: .mx-auto.d-block :}
##### _Words and Curses can have a profound and lasting impact on the lives of those targeted._

#### How to respond to Advanced Persistent Curses?

It's important to start by examining your personal space for hex bags (or putsi bags) and other cursed objects, and burning them. Consider looking under the bed, in your car, office or other area you spend a lot of time for the cursed objects.

Defensive hexes can be used to mitigate or counteract the negative effects. Things like Cedar, Cinnamon, Mint, Nutmeg and Sage are powerful ingredients that can be used in charm bags, vials or worn on the body.

I urge you to consider the aura of your supply chain.

Note to recruiters: I do not wish to be contacted at this time.
