---
layout: post
title: Metadata Analysis of php-src backdoor attempt
subtitle: Investigating future model detection mechanisms for open source project repositories
toc: true
#cover-img: /assets/img/solarwindsceocv.png
thumbnail-img: /assets/img/post4/commit.png
share-img: /assets/img/post3/commit.png
tags: [supplychain, sdlc]
---

#### Background
This work follows on from from  my [prior metadata analysis](https://5stars217.github.io/2021-05-03-metadata-analysis-flatmap/) which provides justification for this type of analysis.

The PHP project [recently discovered](https://github.com/php/php-src/commit/2b0f239b211c7544ebc7a4cd2c977a5b7a11ed8a) that attackers were able to gain access to its main Git server and upload two malicious commits, one of which contained a backdoor. Thankfully, the backdoor was discovered before it went into a release.

Both commits claim to address a “typo” in the source code. They were uploaded using the names of PHP’s maintainers, Rasmus Lerdorf (one of PHP’s co-authors) and Nikita Popov.

![pic of malicious commit](/assets/img/post4/commit.png){: .mx-auto.d-block :}  

The commits were caught by other maintainers:

![pic of maintainer discovering commit](/assets/img/post4/phpdiscovery.png){: .mx-auto.d-block :}  

As somebody interested in bypassing peer review protections and submitting malicious code, this event stuck out to me, what if anything, raised alarm bells in the maintainers who made the discovery _before_ they read the code?:

####   _Was this a long tail event that could be alert-able at scale?_


For my modeling of this event, I wanted to see using only metadata of an actionable intelligence postmortem could be achieved with as little context regarding the malicious event itself as possible.


  After all, if that wasn’t the case, how could it ever deliver actionable intelligence in the real world for future predictions?  I can’t have each modeling just morph to fit every prior event if I expect to make a novel detection in the future. The alert pushes to humans need to be high confidence to begin with, even if that means missing things.


I planned to talk to [Michael Voříšek](https://github.com/mvorisek) the PHP maintainer who discovered the backdoor attempt when I was done with my model. My intent in reaching out was I wanted to find out if there was anything that rung alarm bells or felt unusual _before_ he looked at the commit code itself. I had  (meta)data associated with the backdoor attempt, and certainly some things looked unusual, but without Michael’s context I couldn’t be sure if it actually felt that way up close.
Michael has a long history and is close to the project, and it cannot be discounted the innate and immeasurable value of this also. I just wanted to see if the data could provide some of that context or awareness to mere mortals.

#### Modeling the backdoor attempt

I established the following unusual activity about both the commit, and committer @rlerdorf:

-rlerdorf last committed to `PHP-src` in Oct 23 (by accident), prior to that, May 21, 2019, and that it took place at an unnusual local time that didn't follow prior patterns.
-Wrote ‘ fix typo’ but the malicious commit was large, larger than other commits in a 12 month period by a significant margin.


Here is a comparison of the average commits vs this malicious event:

##### Malicious Event:

`message_analyzed
[skip-ci] Fix typo
Fixes minor typo.
Signed-off-by: Rasmus Lerdorf <rasmus@lerdorf.com>`


![pic malicious commit stats](/assets/img/post4/thebadcommit.png){: .mx-auto.d-block :}  

##### Normal Event Example:

`message_analyzed
Fix typo
Closes GH-6708
Signed-off-by: George Peter Banyard <girgias@php.net>`

![pic normal commit stats](/assets/img/post4/normaltypofixcount.png){: .mx-auto.d-block :}  

Both are unsigned.


The largest commits behind the malicious ‘fix typo’ was for branch merges listed as being part of a feature release in 7/2020!
This is pretty significant.

![size of fix typo](/assets/img/post4/fixtypocomparo.png){: .mx-auto.d-block :}  

Less significantly, this malicious commit was also unusual activity for the local time for commit author, although I don’t put too much weight in this being super useful in the future, it is a nice potential freebie weighting.

![author commit frequency spread](/assets/img/post4/authorcommits.png){: .mx-auto.d-block :}  

### Determination:

This unsigned commit was labelled as a ‘quick fix’ for a typo but was huge, and from a non-consistent maintainer working outside of normal hours.
Obviously the commit content itself was bad, but I am focused on macro-scale metadata analysis which is language agnostic.

   -Positively, around half of commits since the event are now signed, a large increase in  the right direction.

#### What did [Michael](https://github.com/mvorisek) have to say?

Michael was kind enough to reply, and even suggest some great techniques for other detection models; whilst cautioning about overall high rates of false positives I’m likely to run into throughout this project.

Michael wrote :

>”In that particual php-src commit, "fix typo" definitely caught my attention as this wording is never used to fix more than cosmetical changes in such large project.”


So basically, after seeing the commit size and the description/summary, Michael was already at attention!

This is great news for scaling up this particular detection method moving forward I think, by combining a couple more of the risky or anomalous data points, we can certainly see unusual activity hopefully keep the false-positives under control.

I plan to create additional detections based upon normalized 'fix typo' or 'hotfix' style commits leveraging this information.

#### Unsolved issues

Drive by or ‘infrequent’ contributors are a tricky problem in this space, that I don’t feel like I have a good solution for yet. I think the weightings will have to be based on project-specific data.  Here’s an example of a project with a large number of drive-by contributors that might surprise some:


`Ethereum-src`:

![eth has a lot of driveby contributors](/assets/img/post4/ethdriveby.png){: .mx-auto.d-block :}  


#### Final Notes


Now is a good time to talk about commit signing. Without commit signing, it is possible to present commits as if they have come from another user.
This is a problem with both enterprise or self hosted repos,  but also public ones. Impersonating another well regarded committer is an excellent way to mask your progress, especially in teams where peer review is not culturally ingrained as a net-positive.

It would be great if in the future, signing commits becomes the enforced norm.
