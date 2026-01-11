# TPU BASICS 

this file contains all the short notes and all that stuff in this one file 

## [google cloud blog](https://cloud.google.com/blog/products/ai-machine-learning/an-in-depth-look-at-googles-first-tensor-processing-unit-tpu)

neural network mostly have these three operations 
1. multiplication : $w*x$
2. addition : $\sum{w_i*x_i}$
3. activation functions : ReLU, GeLU, Sigmoid etc 

most of the computation is needed for the multiplication.

first optimization: GPUs or CPUs mostly work with ordinary 32-bit or 16-bit floating point operations, but to use TPUs **QUANTIZATIONS** is used - this enables us to reduce the total amount fo memory and computing resources required

the TPU includes the following computational resources:

- Matrix Multiplier Unit (MXU): 65,536 8-bit multiply-and-add units for matrix operations
- Unified Buffer (UB): 24MB of SRAM that work as registers
- Activation Unit (AU): Hardwired activation functions

the heart of the tpu is the SYSTOLIC ARRAY 
- A systolic array chains multiple ALUs together, reusing the result of reading a single register.

![TPU system architecture](images/tpu-system-architecture.png)