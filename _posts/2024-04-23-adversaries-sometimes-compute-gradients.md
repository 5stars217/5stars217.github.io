---
layout: post
title: Adversaries sometimes compute gradients. Other times, they rob you.
subtitle: Build your adversary flywheel. 
toc: true
#cover-img: /assets/img/solarwindsceocv.png
thumbnail-img: /assets/img/post11/flywheel.gif
share-img: /assets/img/post11/flywheel.gif
tags: [ml,sdlc,red team]
---


# Adversaries sometimes compute gradients.

## Bottom line up front:
You want to know where the defender has less visibility, and exploit that?

Build an adversary flywheel.

The next phase of asymmetric adversarial engagements against apex defenders requires you to understand the defensive flywheel, and use data science to rapidly pivot the rotational axis of the much smaller and nimble adversary flywheel.

## The adversary flywheel?

**What is the adversary flywheel?**

Encompasses tools, tactics and processes where an attacker saves telemetry from their operators, collects telemetry about defensive tools, and iterates on data gathered from defensive machine learning models via any means within their scope of authorization (or lack thereof…).

They use this for data driven decisions regarding attacks, evasion and exploitation to better model the behavior and motivations of sophisticated threat actors, some of whom leave little to nothing to chance. 

Building an adversary flywheel gives red team operators more time to surface vulnerabilities that are unique to their target environment and less time validating vendor purchase decisions, whilst taking a forward looking stance.

The term is borrowed from the 'machine learning flywheel':

> What is a flywheel? I believe the term in ML originated from Geoff Hulten [here](https://link.springer.com/video/10.1007/978-1-4842-3933-9/) describing the process of where a good, clean, data pipeline incoming from users creates better models, which attracts more users and creates better models. 
> 
> Like the flywheel in a car storing energy in the form of rotational momentum.

In a security context, vendor defensive tooling has long-term access to all the attack (and benign!) telemetry of multiple customers. That's a lot of momentum being stored in that flywheel, leading to more opportunities for detection.  Their flywheel creates a complex decision function encompassing static, dynamic, signature and ML checks across a range of security verticals. 

<img src="/assets/img/post11/flywheel.gif" style="width: 50%; height: auto; float: right; margin-left: 20px;">

A bigger flywheel with more momentum has its disadvantages though; it cannot change its rotational axis quickly, and that can be used to our advantage, allowing attackers to navigate a complex topography much faster. 

Furthermore, changing complex systems has downstream consequences which are often hard to measure or further increase cost.

The story of attack & defense asymmetry lives on.

<div style="display: flex; justify-content: center;">
  <div style="flex: 1; margin-right: 10px;">
    <img src="/assets/img/post11/objective.jpeg" style="width: 100%; height: auto;">
    <p style="text-align: center;">Isn't that the objective?</p>
  </div>
  <div style="flex: 1; margin-left: 10px;">
    <img src="/assets/img/post11/depends.jpeg" style="width: 100%; height: auto;">
    <p style="text-align: center;">Depends where you're standing</p>
  </div>
</div>

Put another way, when assessing AI/ML capabilities in attack/defense, would you rather navigate the landscape on the right in the more nimble flywheel or the one that changes direction more slowly? Inspiration taken from understanding complexity: ['simple, rugged and dancing landscapes'.](https://www.youtube.com/watch?v=3FyzOba2cUE&t=3s) People often make the mistake of assuming their business landscape and an attackers goals within it are like reaching the peak of Mount Fuji, but often its more like navigating the Appalachias, where its hard to judge where the peaks are from the different vantage points. 

## Building my adversary flywheel

### Step 1: Create a data flywheel 
Attackers need to use more ML in their day to day. To really do that, they have to start building their flywheel and using ML adversarially and offensively. 

Not just because blue teams are doing it, but because true adversaries are heavily invested in the space. When we look beyond 'cyber-criminals'  we see that there are adversaries  with the backing of multiple universities, dedicated ML teams and research teams. They take an active interest in understanding how to use ML both adversarially and offensively.  For an actual well thought out take on reevaluating attacker capabilities, and if this approach is right for you, see ['are we really helping'](https://jackson-t.com/are-we-helping/) by the venerable [Jackson-t.](https://twitter.com/jackson_t?lang=en)

To start, you need a data flywheel.  Projects like [red team telemetry](https://github.com/ztgrace/red_team_telemetry), [redELK](https://github.com/outflanknl/RedELK) and  [nemesis](https://wiki.offsecml.com/Offensive+ML/Flywheels/Nemesis) (a red team 'flywheel') which is enabling red teams to begin to build a database of attack telemetry for future use, like in ML or for static / dynamic evasion techniques, and creating a data pipeline for analysis and so on. But that's just one piece of the puzzle; we need more ML driven data inputs and techniques in play.

### Step 2: Use defensive systems offensively

Systems that use ML are a valid target, both closed and open source.

An important sophistication leap is possible by using AI/ML defensive technologies offensively to [generate better phishes](https://wiki.offsecml.com/Offensive+ML/Phishing/Avoiding+phishing+webpage+detectors+via+black+box+ML), malware, and more by [using models intended for defensive purposes.](https://github.com/elastic/ember) Enabling surgical attacks using data science and evidence, rather than intuition by extracting relevant features to understand why something is flagged bad and modifying the maliciously flagged components based on lessons learned.

Your immediate criticism of this might be "but ml is just one layer in a highly complex decision function" which is true, but if I'm already employing evasion tactics for signature, static and dynamic analysis, why wouldn't I do this too? 

We see in the following example diagrams for phishing detection and anti virus detection that ML detections are just 1 small component of the detection stack. Fixating on that won't get you very far on its own. 


<div style="display: flex; justify-content: center;">
  <div style="flex: 1; margin-right: 10px;">
    <img src="/assets/img/post11/phishing.png" style="width: 100%; height: auto;">
    <p style="text-align: center;">phishing workflow simplified</p>
  </div>
  <div style="flex: 1; margin-left: 10px;">
    <img src="/assets/img/post11/malware.png" style="width: 100%; height: auto;">
    <p style="text-align: center;">av workflow simplified. In both cases, ML is just a component of the detection stack.</p>
  </div>
</div>

However, avoidance of things like signature checks, and static analysis techniques is par-for-the-course in most C2 frameworks, which perform things like symbols obfuscation and generate unique payloads by default. LLM hackbots can also generate content that bypasses these checks trivially. With that done, you can then think about what needs to be done to survive contact with a ML detection engine.

We see in the activities of Advanced Persistent Threat's(APTs) that they are highly motivated to [compromise security researchers](https://therecord.media/microsoft-warning-svr-russia-breach-stolen-information) and learn about the defensive posture of an organization. They are not interested in leaving things to chance, so they learn about, or steal your defensive ML systems. They don't necessarily have to  ['compute your gradients'](https://arxiv.org/pdf/2212.14315.pdf), they steal your shit through whatever means possible ([relevant xkcd](https://xkcd.com/538/)).


Below them in the predator hierarchy is a range of attackers ranging from the sophisticated to not, who are also able to gain access to this type of data through a variety of means. All the more reason to integrate these approaches into your work.

So lets talk about the application of ML for red team purposes - offensive ML. How is it currently being done or discussed publicly?

## State of Offensive ML

Since [Conference for Applied Machine Learning in Information Security](https://www.camlis.org/) (CAMLIS) 2023 [I've been frantically trying to understand the offensive ML landscape](https://wiki.offsecml.com), contribute to it, and evangelize it. Because I think ML is a underrated attack surface.

As I mentioned, defensive phishing web page detectors are being used [adversarially to generate better phishes](https://wiki.offsecml.com/Offensive+ML/Phishing/Avoiding+phishing+webpage+detectors+via+black+box+ML) by taking open source phishing webpage detection models and building phishing web pages against their features. Let's examine that a little closer:
Using the spacephish dataset and code by [Biagio M](https://github.com/biagiom), we'll train a model, and use it to generate HTML features and insert them into phishing pages and see if we can make the models confidence of phishing go down:

![](/assets/img//post11/spacephish.png.png){: .mx-auto.d-block :} 

<iframe src="https://capture.dropbox.com/embed/Erf6NvSjUeQBlK9k?source=copy-embed" width="560" height="315" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

What do we learn?  We learn that adding HTML attributes designed to assist vision-impaired users to phishing web pages to lower detection rates:

![](/assets/img/post11/usaa.png){: .mx-auto.d-block :} 

The end result is a phishing web page designed with data science, not just black magic and communal knowledge. 

The same logic can be applied to malware detection and other defensive ML verticals: 

![Source: [endgame-ml-features](https://www.elastic.co/jp/blog/opening-machine-learning-black-box-model-interpretability) calculation using [SHAP](https://christophm.github.io/interpretable-ml-book/shapley.html) values. You can see the features 'pushing' a detection, this can be used to refine payloads.](/assets/img/post11/endgame.png){: .mx-auto.d-block :} Source: [endgame-ml-features](https://www.elastic.co/jp/blog/opening-machine-learning-black-box-model-interpretability) calculation using [SHAP](https://christophm.github.io/interpretable-ml-book/shapley.html) values. You can see the features 'pushing' a detection, this could be used to refine payloads.

I have been operationalizing detection data and performing [behavioral analysis of endpoint detection systems using unsupervised learning](https://www.youtube.com/watch?v=DLDrsSb_iqI&t=4s).
These offensive anomaly detectors can inform us when a defensive technology has detected something it cares about and build us an ongoing database of telemetry. It isn't necessarily how a sophisticated threat actor would  collect this data (illegally) but it creates a legal mechanism to simulate it, since we aren't reversing the software either. 

 - In this manner, control evasions can be developed not by intuition or historical knowledge, but by understanding the specific ML features or labels of a detection , and adjusting accordingly.

More recently, adversarial research to avoid [netflow classifiers](https://wiki.offsecml.com/Offensive+ML/Attacking+Netflow+Classifiers/Poisoning+netflow+classifers) by back-dooring or poisoning classifiers  has been of keen interest to Advanced Persistent Threats(APTs). Some of these APT's have university backing in AI/ML Research, huge budgets and many experts, allowing them to add high end ML evasion techniques to their tool kits.

Concepts like gradient descent and transferability mean that even black box closed source models could fall victim to these approaches even where the attacker doesn't have direct access to the relevant model, using a close open source alternative can be good enough for some tasks and is within reach to small teams. There's only so many 'features' that can be attributed to a html page or a binary.

> Transferability:
> This is the idea that closed and open source models of the same generation, which share many of the same data points can be fooled the same way, we see this often and very tangibly with image based attacks and LLM based prompt injection attacks which frequently transfer across models since the datasets and extractable features often overlap.

You can see the data I've collected on that topic within the [Offsec ML wiki](https://wiki.offsecml.com/), showing the transferability of adversarial attacks across models for each attack:

![](/assets/img/post11/transferability.png){: .mx-auto.d-block :} This was an easter egg of the site for some time. 


Consider that every ML tool or dataset in the 'defensive' AND 'adversary' category of the Offsec ML wiki, is potentially an adversarial playground for crafting better attacks with data science. 
For instance, open source malware models can be used to study and modify the qualities of a portable executable(PE) that flagged it in the first place. (This idea for executables is not original to me but I have been asked to not cite the person I would attribute it to). This won't result in evasion of non ML based detection, but it removes a lot of guesswork.

> The data for adversarial phishing bypasses shows that adding accessibility features to your phishing web pages drastically lowers the classification of it as phishing. What might you learn to apply this to other things? ;)

Another great example  that comes to mind is ['proof pudding'](https://wiki.offsecml.com/Offensive+ML/Phishing/using+model+inversion+to+reverse+engineer+phishing+detectors) by [moo_hax](https://github.com/moohax) - where a black box model was 'inverted', allowing for adversarial use by the researcher who leveraged access to certain data-points used by the model, allowing them to bypass the security control through their newfound intimate knowledge by replicating the defensive model offline, totally removing the guesswork.

This didn't directly give the operator 'access' but it did enable anyone who did this technique to intimately understand what the model cared about and would flag on. - What a confidence boost, operational security improvement and time saver to enable them to spend time on things that matter.

Other offensive ML techniques that are possible with good system telemetry, such as  [DeepDrop](https://silentbreaksecurity.com/blog/technical/adversary-simulation/machine-learning-for-red-teams-part-1/) by moohax, a pioneer in the space. A dropper designed to use ML to decide if a second stage should be downloaded to the environment, or a method in MeatPistol by ceyx for generating sensible payload names relative to the directory its being installed in are all examples of using telemetry gathered from hosts over time to understand them and improve attacks.

The cryptominers are another canary in the coalmine here, who have recently begun to [target ML pipelines due to their powerful GPUs](https://www.oligo.security/blog/shadowray-attack-ai-workloads-actively-exploited-in-the-wild#18), and use their insane level of access in systems like Ray to … mine coins. Mostly ignoring that these platforms would allow them to steal models, datasets, and conduct poisoning attacks.

Therefore, it is entirely valid and common sense right now for a phase of an operation to include a 'smash and grab' focused only on the defenders telemetry systems to better fine-tune the next phases of the attack using data science. In the same way that attackers are known to just straight up purchase or steal vendor defensive products to study them. Typically this is out of reach for internal red teams, and in and of itself not a useful goal vs studying unique aspects of your environment, but integrating telemetry collection is!

Yes, this is just one facet of a complex web of signature, static and dynamic technologies, but if you have an opportunity potentially remove a variable and not leave something to chance, why aren't you?

## On better usage of operator telemetry as a result of flywheel development

One of the biggest limiting factors  in the creation of 'hack bots' thus far  has been a lack of good telemetry - [as mentioned in this talk](https://www.camlis.org/cheng-wang-2023). A range of reward mechanisms and other techniques have been tried to create hack bots, they continue to be limited in their ability to prioritize and perform long term actions. In other words - there is no pre existing flywheel. You can see some interesting bench marking on LLMs cyber security strengths and weaknesses [here](https://www.camlis.org/adarsh-kyadige-2023) (point in time, maintain a soft opinion on capabilities).  

1. Attack is already asymmetrical to defense. One skilled operator can generate hours or even days of work for a team of incident responders in a matter of minutes, since they can front-load so much of their activity.
    - LLMs working as agents ('hackbots') are beginning to [benchmark as novice operators](https://arxiv.org/pdf/2402.11814.pdf) and hack independently in 'no-bs' studies(and plenty of bs ones); a transformation in skills over the last 12 months. Right now, an operator could use a novice LLM hack bot as a form of 'operational denial of service' attack, which in turn transforms the volume and frequency of attacks that could be experienced. 
        - Hackbots on endpoints are limited in their success vs web app hackbots, because 1000 probing web requests does not move the detection and response needle severely the way 1000 endpoint probing requests would.
        We can expect the next leap in LLMs sophistication to rival junior to mid level operators and further broaden this capability and carry out attacks and scanning independently, which will be a significant problem for companies with immature security controls who could be overwhelmed or straight up victimized in this way.

Having telemetry about what is unique to your environment will be key in having success with these tools. Do not hold strong opinions on the capabilities of Large Language Models or ML for hacking, always be prepared to pivot and embrace the latest research. 

Myself and others see that APT's are already using AI/ML heavily, and the threat intelligence backs it up, [sophisticated adversaries are backed by universities](https://www.concentric.io/blog/the-global-landscape-of-ai-development-china-russia-and-irans-strategies-and-impacts), own their own supercomputers, target and hire AI/ML researchers, have AI research in their charters, and are highly motivated to remain stealthy. 

If that's your goal, to simulate their behavior, it requires having offensive and adversarial ML techniques in the toolkit, because [the ML models are attack surface in and of themselves](https://skylightcyber.com/2019/07/18/cylance-i-kill-you/) - most of which can only be gained by learning from open source defensive ML sets and gathered telemetry or via inversion or anomaly detection techniques.
Caring more about the data you collect and what you do with it is the first step on this journey. 

I expect that those who take this approach will experience sense of return to an earlier period of computer security where attackers experienced higher variance in the efficacy of defensive technologies, and the 'adversarial flywheel' will be a driving force behind this.

## How to incorporate an adversary flywheel into your workflow and thinking:

- Stop deleting everything after every engagement, keep telemetry. You'll need it for offensive Machine Learning tasks. 
  
- A straightforward way to do adversarial ML is using defensive datasets and models and the properties of transferability to "know-before-you-go". Use data science to fine-tune and augment your evasion approaches.

- Defensive tools are valid targets and contain information you need to emulate sophisticated adversaries, just be careful to balance this with where you can provide the most value; in the places unique to your environment.

