# Overview

https://docs.xilinx.com/r/en-US/ug1414-vitis-ai/Vitis-AI-Overview

Optimize custom DL models (especially CV ones) by three steps, namely

- *pruning*: simplify the models by deleting weights or/and neurons with minimal performance loss possible
- *quantization*: (partially) transform `float32` model weights into `int8` with minimal performance loss possible
- *compilation*: compile the pruned and quantized models into `.xmodel` files

for [DPU-PYNQ](https://github.com/Xilinx/DPU-PYNQ) enabled Xilinx FPGA boards using [Vitis-AI](https://github.com/Xilinx/Vitis-AI) library.