# [Mixed Presion Training](https://arxiv.org/abs/1710.03740)

## Main Idea

Training a network with reduced precision while maintaining mode accuracy to address models that have large computational and memory requirements

## Working

- <ins>Reduced Precision Training</ins>: The forward and backward propagation of network is performed in reduced presion (FP-16) while a master copy of weights in FP32 format is maintained. The gradients computed in reduced precision is used to update the master copy which are then floored for the next forward pass.

- <ins>Loss Scaling</ins>: Scale the loss value computed in forward pass just before starting backpropagation. The Chain rule ensures that all the gradient values are scaled accordingly by the same factor. This requires no extra operation and keeps relevant gradients from becoming zero. The weight gradients must be unscaled before the update to FP32 weights. It is done right after backward pass and bbefore any clipping operation, ensuring that any hyperparameters such as weight decay and gradient clipping threshold remains same. 

- Reduction operations: Large reduction operations such as batch-normalization are done in FP32 precision. The input and output of such an operation will still remain in FP16 format but the arithmetic operation will be done in full precision. 