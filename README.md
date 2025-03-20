# MIPS-processor
A Verilog implementation of a 5-stage pipelined-processor capable of executing MIPS assembly instructions.

Feliciano Cortes 
EE180 Lab4 Report

Stages: Fetch-Execute-Decode-Memory-Write Back

1. Objective:
The goal is to implement a sobel filter algorithm by designing a hardwared system optimized for this specific function. This differs from lab3 in which we deisgned a processor for general purpose programs. In this lab, we choose to optimize our build for a single purpose (the sobel filter program), which should result in significant performance improvements.

2. Implementation:
The only files of focus for this lab were sobel_control.v and sobel_accelerator.v. The first file is the state manager, and it also specifies the datapaths of our program in each state. As part of the implementation, I drew out a state diagram, which I plan on submitting by email. The second file finishes the implementation of the convolution step of the sobel filter.
sobel_control.v:

* The first step is specifying the transitions between states. This part was simple as we just move to the next state when "go" is high, except for a few exceptions. In PROCESSING_CALC, we want to go to the PROCESSING_LOADS_LAST if we're in the last row of the column, instead of just the regular PROCESSING_LOADS. Similarly, in PROCESSING_CALC_LAST, we want to go to PROCESSING_DONE if we're on the last column, otherwise back to LOADING_1 for the next column.

* The rest of this module maintains the data for row_counter_next, col_strip_next, buf_read_offset, buf_write_offset, and buf_write_en. Each needs to be maintained in their own "case" blocks as I believe we're only allowed to write to one signal per block in Verilog.
sobel_accelerator:

* For this module, I added four signals to help with the convolution: convx_above/below, and convy_above/below. They each held the sum of the pixel data for both x and y, respectively, around the target pixel. Then I took the absolute value difference between the above and below data, and set that eaqual to convx and convy respectively. The sobel_sum is the sum of these two values, with a cap value of hex-FF.

4. Testing & Issues:
I spent the most amount of time with the case blocks and making sure the states were tracking well. A lot of the states had no changes between each other, but at first I just wrote all of them in to make sure my intuition was correct. Afterwards, I tried testing by removing some of the unnecessary states as the starter code suggested, and in most cases, it worked the same. I kept my implementation to two cores for simplicity, and I'm hesitant to try generating a bitstream as the process is still a bit unclear to me. But I'm confident that my implementation works as I made sure all tests are passing.
