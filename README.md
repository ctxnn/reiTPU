# reiTPU - implementing TPU from scratch 

### what is TPU?

a tpu is TENSOR PROCESSING UNIT, a specialized hardware accelerator designed by GOOGLE for ML tasks, more precisely tasks involving neural networks. they are optimized for high speed, low precision arithmetic operations, and are used in various applications. they are optimized around a narrrow set of mathematical operations : massive efficiency gains and less power usage

## GPU VS TPU 
- gpus are more general purposed and on the other side tpus are specifically for the matrix operations purpose
- gpus have streaming multiprocessors, but most of the silicon is wasted when they are used for deep learning so tpus strip away this and replaces streaming multiprocessors with dedicated matrix multiplication engine 
- gpus memory/cache is designed for more general purposes  

## TPU architecture 
- it uses systolic array(matrix multiplication units) it is a grid of small arithmetic units
- MAC units : multiply accumulate step is the most important step in neural networks 
- TPU memory is hierarchical like modern computer memory 
- DATA FLOW : hbm(DRAM) <-> unified buffer(on chip SRAM) <-> MXU(systolic array) 
- SCALAR AND VECTOR UNITS: two more units that compilements the systolic array
- TPU uses XLA(accelearted linear algebra) compiler, everything has to pass through XLA before going to the hardware

