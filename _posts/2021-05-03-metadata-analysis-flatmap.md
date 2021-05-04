---
layout: post
title: Metadata Analysis of flatmap dependency supply chain attack
subtitle: Investigating future model detection mechanisms for open source project repositories
toc: true
#cover-img: /assets/img/solarwindsceocv.png
thumbnail-img: /assets/img/post3/majorversionbump.png
share-img: /assets/img/post3/majorversionbump.png
tags: [supplychain, sdlc]
---

There's been hundreds of software dependency supply chain attacks exploiting a range of vectors in the past, with great effect. [The July  2020 paper by Marc Ohm et al ](https://link.springer.com/chapter/10.1007%2F978-3-030-52683-2_2) describes that on average **a malicious package is available for 209 days.** `(𝑚𝑖𝑛=−1,𝑚𝑎𝑥=1,216,𝜎=258,𝑥̃ =67)` so naturally, any method to reduce this number would be well recieved.

 In this post, I want to document a really interesting detection vector that I am attempting to operationalize using software health metrics from readily available metadata in an attempt to begin a compendium of metadata based detection aids for some attack vectors related to this problem.

This method is not really applicable or intended for typosquatted packages (although a lack of metadata is a signal in and of itself), but more for situations where a threat actor gains publishing permissions or control over an existing repository.

 #### from an attackers perspective, modifying a known good package's source code has several stages in the OODA loop:

**1) Identification of a target**
Not withstanding opportunistic account acquisition (i.e : compromised credentials), projects with low maintainer counts, long periods with no commits, longstanding issues historically make for good targets.
Tooling like CHAOSS, and OSSF Metrics are useful for both attackers and defenders in selecting targets.
**2) Determination of malicious code entry vector**
Social Engineering (working to obtain trusted status on the repositories, or PR's for issues), obtaining publishing rights, and repo takeovers are common entry vectors, and ones we're concerned with here, and closely related to target identifcation steps.
**3) Commit(s) of malicious content**
Once code is committed to the repository, the attacker is exposed to any gating mechanisms, and thousands of eyes on. Lengthy peer review periods or branch protections put the attack at risk, they're incentivized to move briskly through this process so their malicious additions execute, pushing their code through to main as soon as possible.


#### Analysis 1: Event-Stream

[Event-Stream](https://github.com/dominictarr/event-stream) at time of exploit, was used by another 1,600 packages, and was on average downloaded 1.5 million times a week.
The exact circumstances surrounding the event are [well publicized](https://www.zdnet.com/article/hacker-backdoors-popular-javascript-library-to-steal-bitcoin-funds/) but essentially the sole maintainer of the Event-Stream Package (who maintains a large number of JavaScript Packages, and is well regarded), gave publishing rights to an individual who wanted to maintain the module. This kept the repository under the original username, making the change less obvious.
![pic of publishing rights discussion](/assets/img/post3/publishing_rights.png){: .mx-auto.d-block :}
The new publisher added a dependency and a minor version increment to Event-Stream called [Flatmap-Stream](https://github.com/hugeglass/flatmap-stream) which had at the time, 1 commit and no users.  **Flatmap-Stream was targeted in its malicious behavior**, designed to target cryptocurrency wallets.  
![dependency addition](/assets/img/post3/add_flatmap.png){: .mx-auto.d-block :}  
![dependency addition](/assets/img/post3/versionbump.png){: .mx-auto.d-block :}

After a few days, (and millions of installs) the publisher removes the dependency and adds a major version increment, leaving a large number of installs but 'cleaner' looking source code if it were to be inspected.
![dependency addition](/assets/img/post3/majorversionbump.png){: .mx-auto.d-block :}

#### What does the metadata tell us about Event-Stream?
![pic of project commit history dwindling](/assets/img/post3/commits.png){: .mx-auto.d-block :}
The data also shows that the project had not received any updates for a substantial period of time, which was surely a factor in the attackers reconnaissance and target selection.
The metadata shows an unusual trend in reviews at the time: less than half the length of any prior review, coupled with a new user publishing: `committer_name:cbd54bcf956440406bd33139413d956b8ae75a27 `
![pic malicious code commits and reviews](/assets/img/post3/reviews.png){: .mx-auto.d-block :}

These data points don't point to anything untoward on their own, for instance, the pic below shows that it was common for the project to have one-off or 'drive by contributors'(very common)- but together the data starts to paint a picture.
![drive by reviews](/assets/img/post3/driveby.png){: .mx-auto.d-block :}

Of course, once the damage is done, we see a significant increase in issues reported:
![drive by reviews](/assets/img/post3/issuespike.png){: .mx-auto.d-block :}

I'll be studying and posting about other similar attacks, I believe by observing packages from the eyes of an attacker, it is possible to first isolate 'unhealthy' projects, as is being done by [the OSSF Metrics group](https://github.com/ossf/Project-Security-Metrics) - of which I hope to be an active contributor moving forward - and then subjecting the repos to additional interdiction when anomalous activity commences in the future.
