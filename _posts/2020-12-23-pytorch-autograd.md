---
title: Understanding Autograd in PyTorch
tags: ml, pytorch
img_url: /assets/img/pytorch_autograd.png
---

I find it very important to understand what autograd is more than simply viewing as a PyTorch engine that computes gradient automatically for us.
Accurately, autograd is an engine for Jacobian-vector product (JVP).
I will cover two things, cycle of its operation and JVP
### Operation Cycle
Autograd is possible because operations on Tensors are recorded, and a directed acyclic graph (DAG), or more specifically dynamic computational graph (DCG), is craeted along with it.
In a view of graph, nodes are Tensors, and edges are operations.

Tensor holds the following important information:
1. _data_: value a Tensor is holding.
2. _requires_grad_: _VERY IMPORTANT!_ tracks operation and forms backward graph which allows backpropagation.
3. _grad_: stores computed gradient.
4. _grad_fn_: backward function used to compute gradient.
5. _is_leaf_: tells if the node is leaf of DCG

While the importance of other attributes are self explanatory, it seems odd to store _is_leaf_.
However, this becomes an important bit because gradient of Tensor is populated only if _requires_grad_ and _is_leaf_ are set to True.

### JVP
When I was going through [tutorials](https://pytorch.org/tutorials/beginner/blitz/autograd_tutorial.html#sphx-glr-beginner-blitz-autograd-tutorial-py) on PyTorch, I found this part very weird and arbitrary.
```python
v = torch.tensor([0.1, 1.0, 0.0001], dtype=torch.float)
y.backward(v)

print(x.grad)
```
This explains the case when we call _backward()_ function from non-scalar output. 
To understand this, I had to learn PyTorch does not (or cannot) compute Jacobian directly to purse simplicity and efficiency.
It uses JVP instead which simply is an inner product of vector and Jacobian.
Normally, the root of DAG, or DCG, of autograd is an output from loss function, simply a scalar value.
This kind of eliminates a reason to compute Jacobian directly and lets us assume computation of gradient always start from a scalar value.

<p align="center">
    <img src="/assets/img/JVP.png" alt="JVP"  width="30%"/>
</p>

This does not mean we cannot compute Jacobian.
An example by [jdhao](https://stackoverflow.com/questions/43451125/pytorch-what-are-the-gradient-arguments/47026836) gives an intuitive explanation on why and how to directly compute Jacobian.
```python
from torch.autograd import Variable
import torch
x = Variable(torch.FloatTensor([[1, 2, 3, 4]]), requires_grad=True)
z = 2*x
loss = z.sum(dim=1)

# do backward for first element of z
z.backward(torch.FloatTensor([[1, 0, 0, 0]]), retain_graph=True)
print(x.grad.data)
x.grad.data.zero_() #remove gradient in x.grad, or it will be accumulated

# do backward for second element of z
z.backward(torch.FloatTensor([[0, 1, 0, 0]]), retain_graph=True)
print(x.grad.data)
x.grad.data.zero_()

# do backward for all elements of z, with weight equal to the derivative of
# loss w.r.t z_1, z_2, z_3 and z_4
z.backward(torch.FloatTensor([[1, 1, 1, 1]]), retain_graph=True)
print(x.grad.data)
x.grad.data.zero_()

# or we can directly backprop using loss
loss.backward() # equivalent to loss.backward(torch.FloatTensor([1.0]))
print(x.grad.data)  
```
This gives an output,
```bash
tensor([[2., 0., 0., 0.]])
tensor([[0., 2., 0., 0.]])
tensor([[2., 2., 2., 2.]])
tensor([[2., 2., 2., 2.]])
```
This is easy to understand with some math.

$$ \bf x = \begin{bmatrix} 1 & 2 & 3 & 4\end{bmatrix} $$

$$ {\bf z} = 2 \odot x\quad (element\ wise)$$

$$ {\partial \bf z \over \partial \bf x} =  \begin{bmatrix} {\partial z_1 \over \partial \bf x} & {\partial z_2 \over \partial \bf x} & {\partial z_3 \over \partial \bf x} & {\partial z_4 \over \partial \bf x}\end{bmatrix}$$

```python
z.backwardtorch.FloatTensor([1, 0, 0, 0]))
```
Then, this will mean,

$$ {\partial z_1 \over \partial \bf x} =  \begin{bmatrix} {\partial z_1 \over \partial x_1} & {\partial z_1 \over \partial x_2} & {\partial z_1 \over \partial x_3} & {\partial z_1 \over \partial x_4}\end{bmatrix}$$

Thus, outcome will be,
```bash
>>> tensor([[2., 0., 0., 0.]])
```

### Reference
My confusion on how autograd works was greatly solved by [Vaibhav Kumar's blog post](https://towardsdatascience.com/pytorch-autograd-understanding-the-heart-of-pytorchs-magic-2686cd94ec95). My post is a note on what I find important from it and added an example and mathematic explanation that were not covered in the blog. 

[To learn more about autograd mechanics](https://pytorch.org/docs/stable/notes/autograd.html)