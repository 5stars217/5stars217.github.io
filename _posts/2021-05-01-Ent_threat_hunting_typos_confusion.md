---
layout: post
title: 	Enterprise Threat Hunting for Dependency Confusion & Typosquatting
subtitle: The fundamentals once again determine the ease of enterprise response.
thumbnail-img: /assets/img/post2/artithumb.png
share-img: /aassets/img/post2/artithumb.png
tags: [threathunting, sdlc, supplychain]
comments: true
---

This is a collection of advice to aid in detection of dependency confusion and typo-squatting attacks largely aimed at enterprise, where response to such a thing can be tricky.


## Package repositories represent a reliable and scalable malware distribution channel.

#### background
On Feb 9, Alex Birsan released [this research](https://medium.com/@alex.birsan/dependency-confusion-4a5d60fec610)
The research detailed how under certain circumstances, package managers will ‘helpfully’ fetch internal artifacts from public registries. Misconfigured or default package management proxies will also perform lookups to public repositories.
It speaks to a growing problem and set of circumstances almost too long to list; including typosquatting, trojan packages, dependency injection,  package takeovers through lost/stolen credentials or social engineering  and attackers using public registries to stage or pull down tooling or infrastructure.

[Typos are still the most common/preferred attack vector](https://link.springer.com/chapter/10.1007%2F978-3-030-52683-2_2)

![Attack Metrics](/assets/img/post2/attack_type.png){: .mx-auto.d-block :}

#### Repository Manager's response:

jFrog Artifactory & Nexus Sonatype responded by reminding customers to use pattern matching on packages to prevent  users requests being misrouted to the public (internet) registry.

Unfortunately, these solutions do not scale well, if you work in an Enterprise with lots of teams and repositories, maintaining these patterns in an effective a really rough ask, and I’m not the only one who feels this way:
 ![jfrog client](/assets/img/post2/jfrogfeelies.png){: .mx-auto.d-block :}

 Honestly when I found out in February, that we might have to do this, I lost a good amount sleep at the scope of the problem, one of our repository managers' virtual repositories is running north of 1.5 million artifacts.

 Additional features have also since been released for Artifactory, such as `priorityResolution`, which can be used to better organize the resolution order of packages in local, remote and virtual repositories, but only in npm and pypi right now.

 ![jfrog client](/assets/img/post2/priorityresolution.png){: .mx-auto.d-block :}

 The problem for enterprises is, firstly, it assumed that they use every language under the sun, and secondly, they have loose rules or gaps in their coverage of namespace convention.
 You can’t use a exclusion rule effectively in situations like the following scenario:

 Let’s say you set an exclude rule  that looks as such, where Mean Girls == business name.
 ~~~
 com/Mean-Girls/burnbook-project/**
 ~~~
  This would prevent the Repository Manager from searching the public registry for packages containing the namespace ‘Mean-Girls’.
 You’re feeling good, thinking that you’ve got things locked down. Meanwhile, development teams are creating internal packages that follow no specific naming convention, or made up their own.
 ~~~
 com/M-G/exp-project/.
 ~~~

#### packages are often just called whatever - namespace enforcement is a boring hygiene item to most

 Also, consider for pypi, there is only the global namespace, (i.e, the packages can be called whatever).
 And that NodeJS (npm) Supports both. yay, the packages are probably just called whatever.  
 The obscurity of this probably won’t save you either, since package.json file disclosures in applications are common and not regarded to be a priority info disclosure finding in most organizations.

 Which leads to the obvious question, what kind of enforcement or detection do you have on namespace conventions?

 A namespace violation might be also considered without this context to be fairly innocuous and low priority.
 Now you got yourself a killchain and a problem a whole lot bigger and more complicated problem to solve for, but totally doable with the right tooling and educational awareness, over time.


 Detection of circumstances where an attacker might be in the process of triggering a dependency confusion attack can be found with tools and [research kindly released by Schibsted](https://github.com/schibsted/artishock). This helps you map your internal packages vs external packages, regardless of the namespace, so that the correct exclude patterns can be set in the repository manager or alternatively they can be claimed by your team.

This is still certainly a daunting task, especially if you work somewhere with a repository manager(s) that looks a little like this:

![jfrog client](/assets/img/post2/artycount.png){: .mx-auto.d-block :}


This problem is not going away any time soon, neither are spelling mistakes and wrong/default proxy configurations, and herein lies some other ways to get eyes on the problem permanently.

#### proxy logs
Whilst artishock can be run indefinitely, proxy logs can be leveraged to track failed lookups, regardless of the spelling, new development projects, namespace or scale.
Let’s use pypi as an example: When a client attempts to fetch a package, and that package currently does not exist, the public registry will respond with a 404: not found.
Along with using this information to correct the exclude patterns in your Package Manager, you can also use this information to create alerts which trigger if said packages ever start to respond with a ‘200: ok’, indicating that they now exist in a public repo and ought to be investigated!

Psuedo-query:

~~~
Host=pypi.org  //substitute for npmjs.org,rubygems.org, etc

uri_path ".*/simple/(?<package_name>.*)/"  //sub URI for rubygems specific, etc



http_status=200  earliest=-2h

http_status=404  earliest=-45d latest=-1d
~~~

@red teamers, claim them for yourself and hack something :).


### Tip

{: .box-note}
**Note:** your internal copies of pip, gems, nuget, maven/gradle, etc should come preconfigured with the correct  internal repository manager, (not just blocking the public ones and calling it a day) for developers to not only speed up the onboarding process, but reduce the number of calls direct out to public repositories.

#### other remediation notes

_with thanks to the ossf resources_:
- Reference only 1 private feed, not multiple
- Protect your packages using controlled scopes
- Utilize client-side verification features (version pinning/integrity verification)
more at https://github.com/ossf/


#### Tools to look for likely vulnerabilities and measure software health

By collecting, analyzing and participating in open source security metrics, we can better improve projects' security posture, and make better decisions on which projects to implement, which is not just a security issue, but a business risk, technical debt and attrition concern.

These tools can also be used to conduct incident response and detection - more to come in a following post.

![ossf dashboard](/assets/img/post2/ossf_metrics.png){: .mx-auto.d-block :}

![ossf dashboard](/assets/img/post2/metrics_model.png){: .mx-auto.d-block :}
