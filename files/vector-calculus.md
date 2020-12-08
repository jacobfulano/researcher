---
layout: default
---


# A Review of Vector Calculus for Neural Networks

The following notes are meant to review aspects of vector calculus that appear often in neural network research. I provide some cursory definitions and then motivate them with examples. I also try to use Einstein summation notation alongside index notation in order to build intuitions for the various operations. So much of this is about careful bookkeeping!


We can try to be consistent throughout the notes by defining column vectors as:

$$ \mathbf{v} = v_j = \begin{bmatrix}
           v_{1} \\
           v_{2} \\
           \vdots \\
           v_{N}
         \end{bmatrix}$$
where $j$ is a "free index," and row vectors as $\mathbf{v}^\top$. Einstein summation notation makes this distinction clear by defining $v^j$ as a (column) **vector** and $v_j$ as a (row) **covector** (a.k.a dual vectors).

## 1. Operations

#### Vector-Vector
Let $\mathbf{u}^\top$ be a row vector of size $1 \times N$, $\mathbf{v}$ be a column vector size $N \times 1$,

| | Index Notation | Summation Notation | Returns |
| --- | --- | --- | --- |
| $\mathbf{u}^\top \cdot \mathbf{v}$ (inner product) | $\sum_j u_jv_j$ | $u_j v^j$ | scalar |
| $\mathbf{u} \odot \mathbf{v}^\top$ (outer product) | $(\mathbf{u}\mathbf{v}^\top)_{ij} = u_iv_j$ | $(\mathbf{u}\mathbf{v}^\top)^i_j = u^i v_j$ | $N \times N$ matrix |

Note that for the **inner product**, the indices are **shared**, while for the **outer product**, the indices are **not shared**. In addition, note that an index that appears twice is called a "dummy index."

We also notice that there is no need for a summation symbol when using Einstein summation notation - the convention is that if there are two indices that are identical, with one raised and one lowered, summation is implied (but if identical indices are both raised or both lowered, there is likely a mistake!)

#### Matrix-Vector

Let $\mathbf{v}$ be a column vector size $N \times 1$, and matrix $\mathbf{A}$ size $M \times N$. The following operations will return a vector:


| | Index Notation | Summation Notation | Returns |
| --- | --- | --- | --- |
| $\mathbf{A}\mathbf{v}$ | $\sum_j A_{ij} v_j = (\mathbf{Av})_i$ | $A^i_j v^j$ | column vector |
| $(\mathbf{A}\mathbf{v})^\top = \mathbf{v}^\top \mathbf{A}^\top$  | $\sum_j v_j A_{ji}  = (\mathbf{v}^\top \mathbf{A}^\top)_i$ | $v_j A^j_i$ | row vector |

We see here that for matrix $A_{ij}$ the first free index $i$ corresponds to the row, and the second free index $j$ corresponds to the column. In Einstein notation, the upper index of $A^i_j$ is the row, and the lower index is the column.

#### Matrix-Matrix

Let matrix $\mathbf{A}$ size $M \times N$, and matrix $\mathbf{B}$ size $N \times P$

| | Index Notation | Summation Notation | Returns |
| --- | --- | --- | --- |
| $\mathbf{A} \mathbf{B}$ | $\sum_j  A_{ij} B_{jk} = (\mathbf{A}\mathbf{B})_{ik}$ | $A^i_j B^j_k = (\mathbf{A}\mathbf{B})^i_k$ | $M \times P$ matrix |
| $(\mathbf{AB})^\top = \mathbf{B}^\top \mathbf{A}^\top$ | $\sum_j   B_{kj}A_{ji} = (\mathbf{A}\mathbf{B})_{ki}$ | $B^k_j A^j_i = (\mathbf{A}\mathbf{B})^k_i$| $P \times M$ matrix |

## 2. Derivatives

Here is a useful table describing the types of common matrix derivatives we might encounter

| Type | Scalar | Vector | Matrix |
| --- | --- | --- | --- |
| scalar | $\frac{\partial y}{\partial x}$| $\frac{\partial \mathbf{y}}{\partial x}$| $\frac{\partial \mathbf{Y}}{\partial x}$|
| vector | $\frac{\partial y}{\partial \mathbf{x}}$| $\frac{\partial \mathbf{y}}{\partial \mathbf{x}}$| |
| matrix | $\frac{\partial y}{\partial \mathbf{X}}$| | |

When dealing with the derivative of a vector with respect to another vector, there is an ambiguity in how to write the Jacobian. This was often a source of confusion for me.

For vectors $\mathbf{y} \in \mathbb{R}^{N}$ and $\mathbf{x} \in \mathbb{R}^{M}$, the Jacobian $\frac{\partial \mathbf{y}}{\partial \mathbf{x}}$ is a matrix that can be expressed as $M \times N$ or $N \times M$. The convention often used in "numerator" notation, where the first dimension of the matrix (i.e. the number of rows) has the dimension of the numerator (e.g. $N$) and the second dimension of the matrix (i.e. the number of columns) has the dimension of the denomenator (e.g. $M$).

$$\frac{\partial \mathbf{y}}{\partial \mathbf{x}} =  \begin{bmatrix}
           \frac{\partial y_1}{\partial x_1} &  \frac{\partial y_1}{\partial x_2}& \dots & \frac{\partial y_1}{\partial x_M} \\
           \frac{\partial y_2}{\partial x_1} \\
           \vdots \\
           \frac{\partial y_N}{\partial x_1}
         \end{bmatrix}$$

The rest of these notes will use this numerator notation.


| | Index | Summation |
| --- | --- | --- |
| $\frac{\partial \mathbf{Av}}{\partial \mathbf{v}}$ | $\frac{\partial }{\partial \mathbf{v}} \sum_j A_{ij} v_j = A_{ij}$ | $\partial_v A^i_jv^j = A^i_j$|
| $\frac{\partial \mathbf{v}^\top \mathbf{A}}{\partial \mathbf{v}}$ | $A_{ji} = \mathbf{A}^\top$ | $A^j_i$|



How do we arrive at this second result unambiguously?




#### Derivative of an element-wise function

We often come across **element-wise functions** such as `Relu`. An element-wise, vector-valued function $\mathbf{f}(\mathbf{x})$ is defined such that


$$f(\mathbf{x})_i = f(x_i)$$

This is particularly convenient when it comes to getting the Jacobian of this type of function:

$$ \frac{\partial \mathbf{f(x)}}{\partial \mathbf{x}} = \frac{\partial f(\mathbf{x})_i}{\partial x_j} = \frac{f(x_i)}{\partial x_j}$$

$$\frac{\partial \mathbf{f(x)}}{\partial \mathbf{x}} =  \begin{bmatrix}
           \frac{\partial f(x_1)}{\partial x_1} &  \frac{\partial f(x_1)}{\partial x_2}& \dots & \frac{\partial f(x_1)}{\partial x_M} \\
           \frac{\partial f(x_2)}{\partial x_1} &  \frac{\partial f(x_2)}{\partial x_2}  \\
           \vdots \\
           \frac{\partial f(x_N)}{\partial x_1}
         \end{bmatrix}$$

But because we are dealing with an element-wise operator, the derivative is non-zero **only** for the terms where $i=j$. This leaves us with:

$$\frac{\partial \mathbf{f(x)}}{\partial \mathbf{x}} =  \begin{bmatrix}
           \frac{\partial f(x_1)}{\partial x_1} &  0 & \dots  \\
           0 &  \frac{\partial f(x_2)}{\partial x_2}  \\
           \vdots \\
         \end{bmatrix}$$

which is a diagonal matrix $\mathtt{diag}(\frac{\partial \mathbf{f(x)}}{\partial \mathbf{x}})$. We can then write the diagonal as a vector $f'(x)_i$ instead of a matrix!

#### Example 1


Given an element-wise function $\phi(\mathbf{u})$, where $\mathbf{u}=\mathbf{W x}$, what are $\frac{\partial \mathbf{\phi}}{\partial \mathbf{W}}$ and $\frac{\partial \mathbf{\phi}}{\partial \mathbf{x}}$?



#### Example 2

Backpropagation is used when training neural networks in a supervised learning setting to change the weights of a network layer based on the error made by the network during training. Python packages such as sci kit learn (`sklearn`) and PyTorch often define layers as $\mathbf{x}^\top \mathbf{W}$ instead of $\mathbf{Wx}$.

Note that we can consistently use the **numerator** notation when working with Jacobians.

So, given an element-wise function $\phi(\mathbf{u})$, where $\mathbf{u}=\mathbf{x}^\top \mathbf{W}$, what are $\frac{\partial \mathbf{\phi}}{\partial \mathbf{W}}$ and $\frac{\partial \mathbf{\phi}}{\partial \mathbf{x}}$?

#### Example 3


Given an element-wise function $\phi(\mathbf{u})$, where $\mathbf{u}=\mathbf{V W x}$, what is $\frac{\partial \mathbf{\phi}}{\partial \mathbf{W}}$? How do the dimensions work out?




#### Example 4

"Vanilla" Recurrent neural networks (RNNs) often have the following form:

$$
\tau \frac{d\mathbf{h}(t)}{dt} = -\mathbf{h}(t) + \phi(\mathbf{W} \cdot \mathbf{h}(t) ) + \mathbf{\eta}(t)
$$

It is convenient to write this in index notation:

$$
\tau \frac{dh_i(t)}{dt} = -h_i(t) + \phi \Big( \sum_k W_{ik}h_k(t) \Big) + \eta_i(t)
$$

Suppose we want to linearize about the fixed point $\mathbf{h}^\*$. Let $\mathbf{h} = \mathbf{h}^* + \delta \mathbf{h}$


$$
\phi(\mathbf{W}(\mathbf{h}^* + \delta \mathbf{h})) \approx \phi(\mathbf{W}\mathbf{h}^{* }) + \mathbf{\phi'}(\mathbf{W}\mathbf{h}^*) \odot (\mathbf{W} \delta\mathbf{ h} )
$$

Using index notation, this would be

$$
\phi \Big( \sum_k W_{ik} (h^{* }\_k + \delta h_k)  \Big) \approx \phi \Big( \sum_k W_{ik} h^{* }\_k \Big) + \phi' \Big( \sum_k W_{ik} h^*\_k \Big)  \sum_k W_{ik} \delta h_k
$$

Plugging this back into the RNN equation, we get

$$
\tau \frac{d}{dt}(\mathbf{h}^\*+\delta \mathbf{h}) = -(\mathbf{h}^* +\delta \mathbf{h}) + \mathbf{\phi}(\mathbf{W} (\mathbf{h}^* + \delta \mathbf{h})) + \mathbf{\eta}(t)
$$

$$
\tau \frac{d}{dt}(\delta \mathbf{h}) = -\delta \mathbf{h} - \mathbf{h}^* + \mathbf{\phi}(\mathbf{Wh}^* )  + \mathbf{\phi'}(\mathbf{Wh}^*) \odot (\mathbf{W} \delta\mathbf{h})  + \mathbf{\eta}(t)
$$

Note that $-\mathbf{h}^\*$ is *not* cancelled by the term $\mathbf{\phi}(\mathbf{Wh}^*)$.





#### Example 6

"Random Feedback Local Online Learning" (RFLO) is a recent algorithm for supervised training of RNNs that, unlike the standard Backprogagation Through Time (BPTT) algorithm, makes some simplifying assumptions that are biologically plausible.



$$
h_i(t+1) = h_i(t) + \frac{1}{\tau} \Big[  -h_i(t) + \phi \Big( \sum W^{\text{rec}}_{ij} h_j(t) + \sum W^{\text{in}}_{ij} x_j(t+1)  \Big)   \Big]
$$

$$
y_k(t) = \sum W^{\text{out}}_{ki} h_i(t)
$$

$$
L = \frac{1}{2T} \sum_{t=1}^T \sum_{k=1}^{N_y} \Big[ y^*_k(t) - y_k(t) \Big]^2
$$

Taking the derivative with respect to the recurrent weights

$$
\frac{\partial L}{\partial W_{ab}} = -\frac{1}{T} \sum_{t=1}^T \sum_{k=1}^{N_y} \big[  (\mathbf{W}^{\text{out}})^{\top} \epsilon(t)  \big]_j \frac{\partial h_j(t)}{\partial W_{ab}}
$$


we can get a recursion relation

$$
\frac{\partial h_j(t)}{\partial W_{ab}}  = \big(1- \frac{1}{\tau} \big) \frac{\partial h_j(t-1)}{\partial W_{ab}} + \frac{1}{\tau} \delta_{ja} \phi'( u_a(t)) h_b(t-1)  + \frac{1}{\tau} \sum_k \phi' (u_j(t)) W_{jk} \frac{\partial h_k(t-1)}{\partial W_{ab}}
$$


For more details, see the original paper: Murray, James M. **"Local online learning in recurrent networks with random feedback."** eLife 8 (2019): e43299.

#### Helpful Resources

* [Stanford CS224N Notes by Kevin Clark "Computing Neural Network Gradients"](https://web.stanford.edu/class/cs224n/readings/gradient-notes.pdf)
* ["The Matrix Calculus You Need For Deep Learning" from explained.ai](https://explained.ai/matrix-calculus/)
* ["A Primer on Index Notation" by John Crimaldi](http://pages.erau.edu/~reynodb2/ep410/Crimaldi_2006_Index_Notation_C.pdf)
