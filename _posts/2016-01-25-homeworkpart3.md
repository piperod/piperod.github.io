---
layout: post
title: Features of Modern Processors 2.
description: "Some Features of modern processors."
tags: [C6786, Modern Processors, pipelines]
categories: [HPSC C6786]
image:
  background: triangular.png
---

# Out of Order Execution. (Dynamic Execution)

As it was  described before, the instructions are stored in the registers and they  are loaded from the memory. However, given that nowadays memories are still not quite too fast, is possible that the arguments of such instructions are not available "on time". For this reason *Out of order execution* can execute orders that appear later on the instructions but have their parameters ready. This improves instruction throughput and makes it easier for compilers to arrange machine code for optimal performance. [Hager][1]. 

The basic idea is to keep track of the program order in which
instructions entered the pipeline and for each of these instructions, it maintains a temporary register storage.[Balasubramonian][6] This way if is needed later to perform a different operation using some of the instructions or results obtained previously. 

Using static (in-order) processors the steps that follows usually are:

1. Instruction fetch.
2. If input operands are available (in registers for instance), the instruction is dispatched to the appropriate functional unit. If one or more operands are unavailable during the current clock cycle (generally because they are being fetched from memory), the processor stalls until they are available.
3. The instruction is executed by the appropriate functional unit.
4. The functional unit writes the results back to the register file

Using dynamic processors, the steps are:

1. Instruction fetch.
2. Instruction dispatch to an instruction queue (also called instruction buffer or reservation stations).
3. The instruction waits in the queue until its input operands are available.  
4. The instruction is then allowed to leave the queue before earlier, older instructions.
5. The instruction is issued to the appropriate functional unit and executed by that unit.
6. The results are queued.
7. Only after all older instructions have their results written back to the register file, then this result is written back to the register file. This is called the graduation or retire stage.
[wikipedia](https://en.wikipedia.org/wiki/Out-of-order_execution#Basic_concept)


Avoiding the stalls on the processors helps improve considerably the performance. This [graph](http://renesasrulz.com/doctor_micro/rx_blog/b/weblog/archive/2010/05/17/pipeline-and-out-of-order-instruction-execution-optimize-performance.aspx) would help us understand why, 

<figure class="center">
    <img src="/images/dynamic1.png" alt="" width="500">
    <figcaption>Static vs Dynamic scheduling</figcaption>
</figure>
Taken from: [wright][8]

In larger operations the improvement would impact higher the performance. Cur- rent out-of-order designs can keep hundreds of instructions in flight at any time, using a reorder buffer that stores instructions until they become eligible for execution[1].