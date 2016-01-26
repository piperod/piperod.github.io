---
layout: post
title: Features of Modern Processors.
description: "Some Features of modern processors."
tags: [C6786, Modern Processors, pipelines]
categories: [HPSC C6786]
image:
  background: triangular.png
---

Despite the limitations we have discussed in the previous post, in the recent years there has been a great focus on chip design looking to improve some features that let us take the most of them. 

## Pipelines. 


Let us suppose you have a shoes factory. Every shoe takes 2 days in construction if its made by one employee. There are a houndred employees at your factory. You want to have every worker doing his job everytime so you don't loose money. You decided then, to have every one working on a new shoe by themselves. In this way, every one start today (let's say monday) and the shoe would be  ready by wednesday. Working in this way is inefficient, because  you have to wait two days to have shoes ready for distribution. This is the way processors worked before. 

<figure class="center">
    <img src="/images/assemblyline.png" alt="" width="500">
    <figcaption>Assembly lines</figcaption>
</figure>

Taken from here: [Assembly lines](http://middleschoolfalconnews.weebly.com/uploads/1/4/6/4/14644690/3871284_orig.jpg)

Now, let us say you call your friend who is a computer scientist. He then counsel you not to have everyone working on their own shoe, but having them in a line working for different part of the shoe. So that one worker would focus only on pasting the shoes, other in cutting the leather, other one to paint, other one to pack, so on and so far. In this way, you will have a consistent production that produces shoes everytime. This is the way processors are working today. 

Of course there are some considerations. For example, every worker should finish his job on time, in case that one worker delays the whole production would be delayed too. 

Processors manage different instructions for example loads, stores, address calculations, instruction fetch and decode, etc. These instructions can be executed independently,so that they can be  distributed on different registers . Like the assembly line, pipelining takes care of having ready data for processing once the operand has finished. 

For instance, in the most basic schema "fetch-decode-execute" pipeline, while an instruction is being executed, another one is being decoded and a third one is being fetched from instruction (L1I) cache.[hager][3]. In the assembly line, the first product is the one who takes the most time, because the worker situated at the end of the line must wait untill the production moves up to him, in the processor this is the so called winds-up phase. 


>Wind-up phase: This is the latency untill the pipeline is ready to execute the orders simuntaneously. 



In the case  workers need to work on a different shoe, they must need to wait untill the assembly line is empty, to start over, this is the so called winds-down phase. 


>Winds-down phase: This is the latency untill pipeline executes the last order to have the result ready. 


The following image depict this schema more clearly. 

<figure class="center">
    <img src="/images/windphase.png" alt="" width="500">
    <figcaption>Schematic view of a pipeline processing in 2D with 4 processors</figcaption>
</figure>

Taken from [Deserno et. all][4]. 

As shown in the previous figure. CPU 1,2,3 need to wait untill the CPU 0 has the calculation  ready, to move into CPU 1, now CPU 0 and CPU 1 are working simuntaneously. Once each of them has finished (winds-up), the computation time would turn from 4 cycles to only 1 cycle per operation. In the end, we should wait untill CPU 3 finishes with the last calculation (winds-down). And the full instruction would be fully executed[4].

In general for a pipeline of depth m, executing N independent, subsequent operations takes N+m-1 steps. While in a general purpouse takes mN steps. The versus speed up is given by the formula, 

$$ \frac{T_{seq}}{T_{pipe}}=\frac{Nm}{N+m-1} $$

## Drawbacks and Hazards

Knowing the advantages of subdividing the process in more simpler ones, one might think it would be suitable to add more stages to perform difficult tasks. However there are some risks known as the *pipeline hazards* basically for the known problem discussed before of the assembling line: It is required that each stage perform its work on time or the whole pipeline would stop. In this way there are at least three different hazards we can think of[kristoffer](http://kristoffer.vinther.name/academia/thesis/modern_processor_and_memory_technology.pdf) [5]: 

* Structural Hazards: occur when hardware, such as the memory subsystem, does not support the execution of certain two instructions in parallel.
* Data Hazards:arise when a needed operand is not ready, because either the current instruction is not finished computing it, or it has not arrived from storage.
* Control Hazards: occur when it is simply not known which instruction should come next.
