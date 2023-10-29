---
layout: post
title: Introducing the Offsec ML Framework v0.1
subtitle: Enabling Red Teams to quickly leverage TTPs on ML infrastructure
toc: true
#cover-img: /assets/img/solarwindsceocv.png
thumbnail-img: assets/img/post9/logo.png
share-img: assets/img/post9/logo.png
tags: [ml, malware, sdlc,red team]
---


# Offsec ML Framework 

A [database of offensive ML TTP's](https://wiki.offsecml.com/Welcome+to+the+Offensive+ML+Framework), broken down by supply chain attacks, offensive ML techniques and adversarial ML.  The framework aims to simplify the decision making process of targetting ML in an organization.


Want to poison an LLM's ground truths? [We can do that](https://wiki.offsecml.com/Adversarial+ML/LLM+Attacks/Using+Access+to+a+Model+Registry/Modify+an+LLMs+ground+truths). Want to put malware in a model and work out how to distribute it? We got the [former](https://wiki.offsecml.com/Supply+Chain+Attacks/Models/Using+Keras+Lambda+Layers) and the [latter](https://wiki.offsecml.com/Supply+Chain+Attacks/Public+Model+Registries/Using+a+Huggingface+Watering+Hole). -- Multiple ways!


Want to understand the state-of-the-art in Offsec ML [flywheels](https://wiki.offsecml.com/Offensive+ML/Flywheels/Nemesis), [droppers](https://wiki.offsecml.com/Offensive+ML/Droppers/Sandbox+detection+using+process+ratios) and [obfuscators](https://wiki.offsecml.com/Offensive+ML/Obfuscators/Obfuscation+using+markov+chains)? 

Or maybe hit an LLM via API endpoint with a repeated character sequences attack? [We got that too](https://wiki.offsecml.com/Adversarial+ML/LLM+Attacks/Using+an++API+Endpoint+or+Black+Box/Prompt+Injection/Using+Repeated+Character+Sequences). 

![gif of graph in action](/assets/img/post9/graph_view.gif){: .mx-auto.d-block :} 


## What's next?

More verified TTPs, more PoCs. The supply chain section is the most fleshed out right now, and the second prioirity is the offsec TTPs. 

Better graphing, right now the graphing on my local works differently to when published: 

Expect to see movement in MLops TTPs and more Supply chain attacks and methods for payload distribution by December '23. 

## More Data

The repo'd copy has more data, namely properties like transferability (True, False, N/A) which tell a really important story; does the listed technique work on other ML models?  Soon this will be available in the graph. 


Enjoy!