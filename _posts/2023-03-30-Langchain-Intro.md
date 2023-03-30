---
layout: post
title: Langchain & Prompt Plumbing
subtitle: One of the coolest things in programming I've seen. Legos for AI.
toc: true
#cover-img: /assets/img/solarwindsceocv.png
thumbnail-img: /assets/img/langchain-prompt.png
share-img: /assets/img/langchain-prompt.png
tags: [ml, langchain]
---

## Background 

After reading about Langchain on a collaborative blog at work, I decided to check it out some. Like most of us, a blocker to my deeper usage of
AI in my workflows is that it doesn’t know about my private data sources, like my Obsidian Wiki, Github Repos, Google Docs and so on.  So the answers aren't often super useful.

I also learned a concept during this time called ‘prompt plumbing’ which I think is a critically important concept in AI/ML that
I will detail at the end.  I think the term ‘ prompt plumbing’ was invented like 60 days ago.
 😅The concept also has significant ramificationsvto how we interact with LLM's moving forward. 

There is a democratizing element to Langchains which gives a user the capability to run and maintain their own selection of private search engines scoped to things they care about.


Langchain has many functionalities, the one I’ve been exploring is the capability to preprocess, split into chunks, and summarize (into a vector space) my private data sources, on my local machine, and remember the context its operating in. 


**In English,** Langchain is software that takes the documents from my personal private sources, like private notes on my machine, and chunks them up in such a way that the relevant data can be taken and ingested automatically into my Large Language Model (LLM), in this case, ChaptGPT questions to improve the response by feeding it relevant context.  This makes chatGPT cheaper, more private(ish), and most importantly, gives functionality on large or recent private data sets which LLM's do not have in their corpus.


There is a public hub of ‘Langchain Loaders’ which is basically ways to ingesting data from various sources to make a more personalized local assistant on your private sources.
You can find the list of [public loaders here](https://github.com/emptycrown/llama-hub).  


The ecosystem of loaders is growing very rapidly, this ecosystem of 'prompt plumbing' is very important for the future of the ecosystem.
Here's another example of the long term usage: 
You provide loaders: such as access to a search engine, sql database, git, and internal documentation tools. 

Then give the AI a *goal*. The agent can iteratively work towards that goal, by itself, retaining memory of the task at hand.
A local memory is a significant advantage that can't be understated, giving long term context to prompts. 

## The proof is in the pudding: 

I asked ChatGPT about something which is not in its corpus, `Bunraku`  - which is the Shadowboxers internal tool for command and control of systems, the documentation of which lives in a private wiki on my machine, note that Im using the GPT3.5 turbo API here, not 4. 

`"what is the purpose of bunraku?"` 


    > The purpose of bunraku is to serve as a C2 (Command and Control) server for ShadowBoxers.

That’s dead on, my prompt is a little … short? but it nailed it. C2’s are also not exactly common terminology outside of my niche. 

So I pointed it at the Github repo containing the code:

`"where can i edit the struct that controls agent internals? include the filename and line number with the function`?


    >The struct that controls agent internals can be edited in the file agents.json located at hack/agents.json. The struct is located on line 3.

Not bad, a great start.

Here's a langchain loader for PDFs:

Lets ask it about the latest [NIST ML guidance from 2023](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.100-2e2023.ipd.pdf) that just came out. ChatGPT isn't aware of this data, so we can provide the document with a langchain loader. 

<iframe src="https://capture.dropbox.com/embed/SyyjrOEWx20WHe0g?source=copy-embed" width="560" height="315" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> {: .mx-auto.d-block :}  



~~~
Yes, numerous attacks against ML models have been demonstrated. For example, poisoning availability attacks have been shown against healthcare and business applications [110]; privacy attacks have been shown against healthcare data [249]; and evasion attacks have been shown against financial applications [90]. Additionally, DeepFool [158], the Carlini-Wagner attack [36], and the Fast Gradient Sign Method (FGSM) [93] have been used to generate adversarial examples against linear models and neural networks. 


Cybersecurity and image classifications were the first application domains that showcased evasion attacks, but ML technology used in many other application domains has gone under scrutiny, including speech recognition [37], natural language processing [115], and video classification [134, 236]. Adversarial examples need to respect text semantics, and FENCE is a general framework for crafting white-box evasion attacks using gradient optimization in discrete domains and supports a range of linear and statistical feature dependencies [53]. FENCE has been applied to two network security applications: malicious domain detection and malicious network trafﬁc classiﬁcation. Sheatsley et al. [196] propose a method that learns the constraints in feature space using formal logic... 
~~~

It goes on. Damn! We can now just interact with the document in a conversational manner. 

The use cases here are nowhere near as sophisticated as the ones being built on the bleeding edge, but there’s still a lot of utility here, for my personal AI assistant project. I’m really thankful to have seen Langchain and to import it into my personal assistant.
Since starting work on this, I’ve learned the term ‘Prompt Plumbing’. I think we should explore that concept some more. 


## Prompt Plumbing 

**Why?**
Because of cost or token restriction reasons you can’t pass an entire corpus of context into the prompt as large prompts are expensive, and the system restricts how long your prompts can be.
You can use Langchain to  preprocess the corpus by iterating over documents, splitting them into chunks, and summarizing them - embed those chunks/summaries in some vector space - when you ask a question, it searches your now preprocessed corpus for similar chunks and passes those relevant chunks to the Large Language Model (openai, in this case, but it doesn’t have to be) in the prompt, along with your question.
This makes your question much better, more detailed and relevant.
 
The use cases above end up being a very common pattern, when interacting with the LLM (in some cases, you might go back and forth with the LLM). For instance, code and semantic search follows a similar pattern (preprocess -> embed -> nearest-neighbors at query time -> LLM).

Langchain provides a great abstraction for composing these pieces. IMO, this sort of "prompt plumbing" is far more important than the often somewhat contrived "prompt engineering" examples we see.

I’m buzzing with ideas of things I want to build with this, as a productivity tool for rapidly understanding things which I don't interact with regularly and rapidly building scaffolding to improve it. 


## Code samples
Some code to get you going on your private sources, in the reference material is some examples of UI wrappers for your personal assistant. 

Here is the code for loading Github’s into langchain:
`pip install langchain` 
`pip install openai` 
`pip install llama_index` 
`export GITHUB_TOKEN=''` 
`export OPENAI_API_KEY=''` 

    import os
    from llama_index import GPTSimpleVectorIndex, download_loader
    from langchain.llms import OpenAI
    from langchain.chains.question_answering import load_qa_chain
    from llama_index import download_loader
    download_loader("GithubRepositoryReader")
    from llama_index.readers.llamahub_modules.github_repo import GithubRepositoryReader, GithubClient
    github_client = GithubClient(os.getenv("GITHUB_TOKEN"))
    loader = GithubRepositoryReader(
        github_client,x
        owner =                  "organization",
        repo =                   "reponame",
    )
    docs_branch = loader.load_data(branch="main")
    docs = loader.load_data(branch="main")
    index = GPTSimpleVectorIndex.from_documents(docs)
    response = index.query("the code contains a number of //todo comments. List them all and prioritize with an explaination")
    print(response)
    #for doc in docs:
    #    print(doc.extra_info)

Here is one for Obsidian 


    from llama_index import GPTSimpleVectorIndex, download_loader
    from langchain.llms import OpenAI
    from langchain.chains.question_answering import load_qa_chain
    import os
    ObsidianReader = download_loader('ObsidianReader')
    documents = ObsidianReader('/working/directory').load_data()  # Returns list of documents
    index = GPTSimpleVectorIndex.from_documents(documents)
    response = index.query("what can i access using xyz tool?")
    print(response) 


Finally, here is one for PDF's. 

    from pathlib import Path
    from gpt_index import download_loader
    from llama_index import GPTSimpleVectorIndex, download_loader
    from langchain.llms import OpenAI   
    from langchain.chains.question_answering import load_qa_chain
    import os

    PDFReader = download_loader("PDFReader")

    loader = PDFReader()
    documents = loader.load_data(file=Path('/Downloads/NIST.AI.100-2e2023.ipd.pdf'))
    index = GPTSimpleVectorIndex.from_documents(documents)
    response = index.query("has anybody ever attacked a machine learning model before? provide some examples")
    print(response)




## References
https://github.com/hwchase17/chat-langchain
no code demo w/ ui: https://github.com/homanp/langchain-ui 