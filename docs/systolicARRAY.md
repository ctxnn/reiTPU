# SYSTOLIC ARRAY 

**formal definition** : a systolic array is a two-dimensional grid of Processing Elements (PEs)—typically multiply-accumulate (MAC) units—connected in a pipelined and synchronized architecture. Each PE performs a simple operation and passes intermediate results to its neighboring elements, enabling highly parallel and efficient computation with minimal memory overhead.

grid like accelerators rhythmically pulse data through a mesh of PROCESSING ELEMENTS.

what we want from the hardware accelerator of AIs:
- parallelism: allowing thousands of operations to execute simultaneously.
- locality: enabling data reuse without repeated trips to high-latency, high-energy memory like DRAM. 

along the FLOPs we also need **arithmetic intensity**(the ratio of compute operations to memory accesses) - we need it because data movement is expensive. fetching data from DRAM can consume orders of magnitude more energy than performing a **MAC**(multiply accumulate)

![image](images/arithmetic_intensity.png)

systolic arrays solve this, because a convulations operation can be restructed as a series of **GENERAL MATRIX MULTIPLICATIONS(GEMMs)**. these GEMMs can be mapped directly onto systolic arrays, which ar specifically designed to computre large matrix products with high throughput and low memory movement. 

$$out = a0*b0 + a1*b1 + a2*b2 + a3*b3$$

you will compute this in hardware like this(this is what a single PE does): 
$psum = 0$
$psum += a0*b0$
$psum += a1*b1$
$psum += a2*b2$
$psum += a3*b3$

![note](A systolic array is just many dot products where inputs flow and weights stay still.
)


**NUMERICAL EXAMPLE:** 
given:
	•	Kernel: 3×3
	•	Input channels: 64
	•	Output channels: 1 (we’ll add more later)
	•	Stride = 1
	•	Ignore batch

convulation formula becomes: 
$$\text{out} = \sum_{ic=0}^{63} \sum_{ky=0}^{2} \sum_{kx=0}^{2} \text{input}[ic][y+ky][x+kx] \cdot \text{weight}[ic][ky][kx]$$

output is a $64 * 3 * 3 = 576* element dot product

flatten: 
$$k = ic*9 + ky*3 + kx$$
a[k] = input[ic][y+ky][x+kx]
w[k] = weight[ic][ky][kx]
out = Σ (a[k] * w[k]) , k = 0..575

in a systolic array:
Row 0   : PE holds w[0]
Row 1   : PE holds w[1]
Row 2   : PE holds w[2]
...
Row 575 : PE holds w[575]

each PE: 
stores one weight
receives one input value
adds to partial sum
passes sum down

array is(when we have more than 1 output channels): 
           oc0   oc1   oc2  ... oc127
        --------------------------------
k0     |  PE   PE   PE   ...  PE
k1     |  PE   PE   PE   ...  PE
k2     |  PE   PE   PE   ...  PE
...
k575   |  PE   PE   PE   ...  PE