# MIPS-processor
Feliciano Cortes 
EE180 Lab3 Report

1. Objective:
The goal is to fully implement a 5-stage-pipelined MIPS processor. It is to be implemented in Verilog and should be capable of executing most MIPS assembly instructions. Most of it is already implemented, we just need to add any missing MIPS instructions and support for hazard detection with correct forwarding and stalling. The five stages are: intruction fetch (IF), decode (ID), execute (EX), memory (MEM), and write back.

2. Implementation:
The main files modified were alu.v, mips_cpu.v, decode.v, and intruction_fetch.v. decode.v contained the majority of the work as it is where we handle jumping and branching, forwarding and stalling, and define a few more arithmetic operations.

* Jumping and Branching:
For these instructions, I added the signals jump_target, jump_branch, and jump_reg to the instruction_fetch module (they were already present in decode.v). These signals allow us to check if we need to do any branching/jumping in IF. 
Then I added a new signal, branch_addr, to communicate any new branch addresses between decode and fetch. I deliberated between calculating branch_addr in decode and sending it to IF, or sending the immediate_unsigned signal to calculate branch_addr in IF. The second option was giving me issues, so I figured the first was the best.

* Forwarding & Stalling:
For forwarding, I added the logic conditions for both the rs and rt registers and from both the MEM and EX stages. For example, we need to forward rs from EX if rs_addr = reg_write_addr_ex, and not equal to zero. Then, I added the proper datapath logic for both rs and rt. For example, if forwarding from EX, we take the data from the ALU, and MEM from reg_write_data_mem.
For stalling, we pretty much just needed to add conditions to check for XORI and jump_reg as they were missing. We were provided the structure for stalling in rs already, so I replicated it for rt as well.

* Arithmetic Operations:
To implement the missing arithmetic operations (XOR, MUL, SRA, etc), I added them to the mux (the case statement) in alu.v using their identifier from mips_defines.v. Likewise, I added them in decode.v, but the calculation happens in alu.v of course.

3. Testing & Issues:
Getting started was the hardest part. I was fortunate to have met with TA Hashem from last quarter as I worked on it this quarter. He helped me understand what files corresponded to what, and what was required of us to complete this lab. 
My first step was just doing a simple make_test to see what's not passing, and then identifying any hazards within a few of these test files. I added no-ops where I saw these hazards, and in may cases the tests passed. And after implementing forwarding/stalling, I commented/delete the no-ops to make sure they still passed.
Hashem also helped me understand how to use make_wave to see my signals. With this, I was able to identify a silly, but important issue. Initially, some of my signals were coming out undefined, and then I realized it was because I didn't correctly connect my new signals in the mips_cpu top-level module. Afterwards, things went pretty smoothly.


