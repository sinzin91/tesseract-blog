---
title: "Book Review - Artificial Intelligence: A Guide for Thinking Humans"
date: 2023-06-26T19:00:32-07:00
draft: false
# weight: 1
tags: ["books", "ai"]
author: "Tenzin Wangdhen"
showToc: true
TocOpen: false
hidemeta: false
comments: false
description: "A primer on AI for laypeople"
disableHLJS: false # to disable highlightjs
disableShare: false
hideSummary: false
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
editPost:
    URL: "https://github.com/sinzin91/tesseract-blog/blob/master/content"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---

Melanie Michell is an AI researcher and professor at the Santa Fe Institute. In her book "[Artificial Intelligence: A Guide for Thinking Humans](https://www.amazon.com/Artificial-Intelligence-Guide-Thinking-Humans/dp/0374257833)," she seeks to make artificial intelligence understandable for laypeople, and for the most part, achieves that goal. She begins by discussing the history of artificial intelligence, starting from the "perceptron" and working up to deep learning for natural language. Michell explains each concept in concrete terms and apt metaphors, with minimal technical jargon and math. Each “AI string”, heralded by new breakthroughs and followed by breathless claims about potential revolutionary applications just around the corner, are inevitably followed by “AI winters”, where the technology falls short of expectations. She also delves into philosophical concepts such as the alignment problem, the singularity, and consciousness in AI systems.

To quote the end of the book:

> _I hope that this book has helped you, as a thinking human, to get a sense of the current state of this burgeoning discipline, including its many unsolved problems, the potential risks and benefits of its technologies, and the scientific and philosophical questions it raises for understanding our own human intelligence. And if any computers are reading this, tell me what it refers to in the previous sentence and you’re welcome to join in the discussion._

One of the challenges of writing a book in such a rapidly evolving field is that it is bound to become obsolete. Mitchell does not cover newer technologies such as LLMs and stable diffusion at all. Furthermore, she makes her stance on AGI risk clear: she thinks that the existential risk from AGI detracts from the real-world consequences of much dumber AI models that are already negatively impacting us today. While I do not fully agree with her philosophical stance, the book provides a great overview of the most important AI models and their history, as well as the philosophical discourse around AI.

## Perceptron

Michell describes the perceptron as a single-layer neural network that outputs a prediction based on input weights. The weights are updated using a "loss function" with the goal of minimizing the loss over multiple iterations to achieve more accurate predictions. Frank Rosenblatt, a research psychologist at the Cornell Aeronautical Laboratory, invented the perceptron. He was inspired by the way neurons process information in the brain, where adjustments to the strength of connections between neurons play a key role in learning. He was also influenced by the work of behavioral psychologist B.F. Skinner, who used rewards and punishments to train animals. In the perceptron, correct firing is rewarded, while errors are punished.

> _To simulate the different strengths of connections to a neuron, Rosenblatt proposed that a numerical weight be assigned to each of a perceptron’s inputs; each input is multiplied by its weight before being added to the sum. A perceptron’s threshold is simply a number set by the programmer._

In 1958, The New York Times reported on the perceptron and predicted that it would "be able to recognize people and call out their names, and instantly translate speech in one language to speech and writing in another." This was one of the first instances of the AI hype cycle that repeats throughout the book and history. However, it was eventually found that the single-layer perceptron could only learn linearly separable problems. The required compute for more layers, and thus more complex problems, did not yet exist. As a result, Marvin Minsky declared perceptrons a "dead-end", and research stalled for decades. This marked the start of the first “AI winter”. Perceptrons are the foundation of the current deep learning revolution, who knows how much we would have progressed had research continued uninterrupted?

> _Reporting on a press conference Rosenblatt held in July 1958, The New York Times featured this recap: "The Navy revealed the embryo of an electronic computer today that it expects will be able to walk, talk, see, write, reproduce itself, and be conscious of its existence. Later perceptrons will be able to recognize people and call out their names and instantly translate speech in one language to speech and writing in another language, it was predicted."_

## Multi-layer perceptron

Multi-layer perceptrons (MLPs) are based on the idea of the perceptron, but are more complex. An MLP has at least three layers: the input layer, the "hidden" layer, and the output layer. The input layer takes in numbers as inputs, the hidden layer processes information from the input layer, and the output layer produces the network's output. The additional layers enable the MLP to learn much more complex functions than a perceptron. An MLP with more than three layers is called a "deep neural network" (DNN).

## Convolutional neural networks (CNN)

Convolutional neural networks, or ConvNets, are machine vision models based on key insights about the brain's visual system, discovered by Hubel and Wiesel in the 1950s and 60s. They found that neurons in the lower layers of the visual cortex are arranged in a rough grid. Each neuron in the grid responds to a corresponding area of the visual field.

ConvNets are deep learning models that transform an input image into a set of activation maps with increasingly complex features, via "convolutions". The lower layers detect things like edges, while higher layers detect more complex features like shapes or faces. The features of the highest layer are fed into a traditional neural network, which outputs confidence percentages for categories known to the network. The network returns the category or label with the highest confidence as the image's classification.

AlexNet was the first to use ConvNets for the 2012 ImageNet competition, achieving 85% accuracy. This was a significant improvement over the previous record of 72%, which was based on support vector machines. The success of AlexNet can be seen as the start of the deep learning revolution. Interestingly, Ilya Sutskever, cofounder of OpenAI, helped create AlexNet.

> *AlexNet, named after its main creator, Alex Krizhevsky, then a graduate student at the University of Toronto, supervised by the eminent neural network researcher Geoffrey Hinton. Krizhevsky, working with Hinton and a fellow student, Ilya Sutskever, created a scaled-up version of Yann LeCun’s LeNet from the 1990s; training such a large network was now made possible by increases in computer power*
    
Although ConvNets are stronger than previous methods, they still have several issues. They can become confused by images that contain multiple objects, miss small objects in an image, and are easily thrown off by slight distortions and abstract representations. Additionally, they have been known to display bias. For example, the Google Photos app infamously tagged an image of two African Americans with the label "gorillas". While CNNs excel at categorization, they still struggle with localization tasks, such as drawing a box around the target object.

> *If the goal of computer vision is to “get a machine to describe what it sees,” then machines will need to recognize not only objects but also their relationships to one another and how they interact with the world. If the “objects” in question are living beings, the machines will need to know something about their actions, goals, emotions, likely next steps, and all the other aspects that figure into telling the story of a visual scene*

## Reinforcement Learning

Unlike supervised learning, reinforcement learning doesn't require labeled data for training. Michell uses the analogy of a dog finding the path to food. The dog (agent), takes actions over a series of "learning episodes", which consist of some number of "iterations". At each iteration, the agent determines the current state and chooses the next action to take. If the agent receives a reward, it has "learned" something. The agent must balance between exploring new paths versus exploiting successful paths. 

> *The promise of reinforcement learning is that the agent—here our robo-dog—can learn flexible strategies on its own simply by performing actions in the world and occasionally receiving rewards (that is, reinforcement) without humans having to manually write rules or directly teach the agent every possible circumstance.*
    
    

DeepMind used deep reinforcement learning to first beat Atari games, and then famously the board game "Go" with AlphaGo. Mitchell describes it as “the ultimate idiot savant”, since it can only play Go very well but that intelligence fails to generalize to other tasks. 

## Natural language processing (NLP)

After the 1990s, rule-based approaches to NLP were overshadowed by more successful statistical approaches that used massive datasets to train machine learning algorithms. Mitchell describes several of the newer algorithms and techniques, including recurrent neural networks (RNNs), long-short term memory (LSTM), and word vectors. 

Word vectors embody the dictum, "you shall know a word by the company it keeps". Words are converted into numbers that are then mapped (embedded) into a semantic space. Here, similar words like "king" and "queen" are closer together than "king" and "basketball". This allows computers to "reason" about the meaning of words, enabling calculations such as "king" + "queen" - "woman" = "prince". This is one of the key ideas behind more intelligent technologies, such as semantic search and ChatGPT.

Mitchell discusses the use of these technologies in tools like Google Translate. While deep learning performs remarkably well at translation, she doubts it will match the expertise of human translators for some time. This is because these technologies lack common sense knowledge and do not truly comprehend the content they are processing.

> *I’m skeptical that machine translation will actually reach the level of human translators—except perhaps in narrow circumstances—for a long time to come. The main obstacle is this: like speech-recognition systems, machine-translation systems perform their task without actually understanding the text they are processing*
    
The book is starting to show its age because it doesn't mention transformers, the most powerful NLP system yet created and used in ChatGPT.

## The Turing test

Alan Turing predicted that in 50 years, computers would be able to trick humans into thinking they are humans about 70% of the time. This became known as the "Turing test". In 2014, a group of Russian and Ukrainian programmers won a competition held by the Royal Society by fooling 10 of 30 judges into thinking it was human.

> *Turing did not specify the criteria for selecting the human contestant and the judge, or stipulate how long the test should last, or what conversational topics should be allowed. However, he did make an oddly specific prediction: “I believe that in about 50 years’ time it will be possible to programme computers … to make them play the imitation game so well that an average interrogator will not have more than 70 percent chance of making the right identification after five minutes of questioning.” In other words, in a five-minute session, the average judge will be fooled 30 percent of the time.*
    
There is now a modified Turing test, designed by Kapor and Kurzweil and to be carried out by 2029. Three humans and one AI will be interviewed by three judges for two hours. The AI passes if it fools two or more judges. Kapor believes that machines will never pass the Turing test without the equivalent of a human body, while Kurzweil believes that AGI is imminent.

> *Mitchell Kapor. He made a negative prediction: “By 2029 no computer—or ‘machine intelligence’—will have passed the Turing Test.” Kapor, who had founded the successful software company Lotus and who is also a longtime activist on internet civil liberties, knew Kurzweil well and was on the “highly skeptical” side of the Singularity divide. Kurzweil agreed to be the challenger for this public bet, with $20,000 going to the Electronic Frontier Foundation*

## The Singularity

Mitchell shares her thoughts on the "singularity": a future where the pace of technological change is so rapid that human life is irreversibly transformed. This idea is closely related to mathematician I.J. Good's concept of an "intelligence explosion", which postulates that an ultra-intelligent machine could design even more intelligent machines, leading to a recursive loop of self-improvement that would leave humans far behind. The resulting gap in intelligence could become as wide as the gap between us and ants. Ray Kurzweil, currently principal researcher, is the most famous proponent of the Singularity. 

> *Kurzweil’s ideas were spurred by the mathematician I. J. Good’s speculations on the potential of an intelligence explosion: “Let an ultraintelligent machine be defined as a machine that can far surpass all the intellectual activities of any man however clever. Since the design of machines is one of these intellectual activities, an ultraintelligent machine could design even better machines; there would then unquestionably be an ‘intelligence explosion,’ and the intelligence of man would be left far behind.*

I was inspired to pursue a career in technology after reading his book "The Singularity is Near" in college. Mitchell makes it clear that she does not believe in the Singularity. She comes close to calling Kurweil a quack, though indirectly.

## Lack of common sense knowledge

One of the core issues with current machine learning systems from Michell's perspective is the lack of real understanding in these algorithms. Despite this lack, she is surprised that language translation has reached such a high level. These algorithms lack "common sense" knowledge, which is something that humans implicitly know and therefore do not explicitly write down. Instead, the machine learns from what it observes in the data, which may not align with what humans would observe. A good example of this is the *Winograd schemas*, which are sentences where changing one word of the question changes the expected answer.

Michell tells the colorful story of "Clever Hans". This horse gained fame because people believed it could "calculate". When asked a question like "What is 5 times 3?", it would tap its hoof the correct number of times. However, it was eventually revealed that the horse did not actually understand math. Instead, it was responding to subtle cues given by the questioner. Michell uses this as a metaphor for something that appears to understand, but in reality, does not understand anything at all. This still holds true today, as even the best LLMs like GPT-4 are still prone to hallucinate, and certain prompts can illicit responses that make it clear that it currently [has no common sense knowledge](https://arxiv.org/pdf/2302.08399.pdf).

## The need for embodiment

According to Mitchell, the primary mode of human learning is experiential, with book learning as an added layer. She argues that AI can never attain a common sense understanding if it does not have a physical presence and experience in the real world. In support of this, she quotes Andrei Karpathy, who suggests that to build computers capable of interpreting scenes like humans, we may need to provide them with exposure to years of structured, temporally coherent experience, the ability to interact with the world, and some form of magical active learning/inference architecture that is currently difficult to imagine.

She quotes Karpathy: 

> A seemingly inescapable conclusion for me is that we may … need embodiment, and that the only way to build computers that can interpret scenes like we do is to allow them to get exposed to all the years of (structured, temporally coherent) experience we have, ability to interact with the world, and some magical active learning/inference architecture that I can barely even imagine when I think backwards about what it should be capable of.

## Predictions

At the end of her book, Michell lists some predictions for some of the most pressing questions in AI. She does not believe that mass unemployment or fully autonomous self-driving cars will happen anytime soon. According to her, creativity is not just about generating content, but also requires the ability to judge and understand what has been created. Therefore, AI is not creative in the full sense of the word. Mitchell also does not believe in an intelligence explosion or the singularity. 

> Above all, the take-home message from this book is that we humans tend to overestimate AI advances and underestimate the complexity of our own intelligence. Today’s AI is far from general intelligence, and I don’t believe that machine “superintelligence” is anywhere on the horizon. If general AI ever comes about, I am betting that its complexity will rival that of our own brains.*

In fact, she states that the opposite of superintelligence is the problem: much dumber AI systems already deployed at scale causing issues such as bias, economic displacement, and increasing radicalization.

> *I think the most worrisome aspect of AI systems in the short term is that we will give them too much autonomy without being fully aware of their limitations and vulnerabilities. We tend to anthropomorphize AI systems: we impute human qualities to them and end up overestimating the extent to which these systems can actually be fully trusted.*

In response to the question of how far away we are from achieving AGI, Michell quotes Oren Etzioni, director of the Allen Institute for AI, who said, "Take your estimate, double it, triple it, quadruple it. That's when."

It's an interesting thought that what are perceived to be human limitations - such as biases, emotions, and cognitive shortcomings - may actually enable humans to possess general intelligence.

## Closing thoughts

Mitchell expresses a healthy dose of skepticism about the capabilities of AI, which may serve as a good antidote to the current massive hype cycle. Given AI's history, it's likely that we'll experience another "AI winter" where the touted benefits never materialize. However, as the book contains the word "guide" in its title, it would be preferable if it were not biased in either direction. While it's important to solve current issues with "dumb" AI, I believe that mitigating existential risks from AI is also necessary. I lean more towards the positive end of the AI doomer to AI enabled utopia spectrum, and believe that AGI aligned with human values may be the last invention humans need to make. Despite this, I would recommend this book to non-technical friends who want an accessible primer on the field of artificial intelligence.
