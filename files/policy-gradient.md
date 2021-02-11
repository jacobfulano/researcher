---
layout: default
---



\begin{equation}
h_i(t+1) = h_i(t) + \frac{1}{\tau} \Big[  -h_i(t) + \phi \Big( \sum W^{\text{rec}}_{ij} h_j(t) + \sum W^{\text{in}}_{ij} x_j(t+1)  \Big)   \Big]
\end{equation}

\begin{equation}
y_k(t) = \sum W^{\text{out}}_{ki} h_i(t)
\end{equation}

\begin{equation}
L = \frac{1}{2T} \sum_{t=1}^T \sum_{k=1}^{N_y} \Big[ y^*_k(t) - y_k(t) \Big]^2
\end{equation}

Taking the derivative with respect to the recurrent weights

\begin{equation}
\frac{\partial L}{\partial W_{ab}} = -\frac{1}{T} \sum_{t=1}^T \sum_{k=1}^{N_y} \big[  (\mathbf{W}^{\text{out}})^{\top} \epsilon(t)  \big]_j \frac{\partial h_j(t)}{\partial W_{ab}}
\end{equation}


we can get a recursion relation

\begin{equation}
\frac{\partial h_j(t)}{\partial W_{ab}}  = \big(1- \frac{1}{\tau} \big) \frac{\partial h_j(t-1)}{\partial W_{ab}} + \frac{1}{\tau} \delta_{ja} \phi'( u_a(t)) h_b(t-1)  + \frac{1}{\tau} \sum_k \phi' (u_j(t)) W_{jk} \frac{\partial h_k(t-1)}{\partial W_{ab}}
\end{equation}





\section{Policy Parameterization for Continuous Actions}

Our general recurrent network (discretized) equation, without input, is:
\begin{equation}
    h(t) = (1-\frac{1}{\tau}) h(t-1) + \frac{1}{\tau}\phi(W \cdot h(t-1))
\end{equation}
$W$ is the recurrent connectivity matrix, and the vector $h(t)$ is the activity at time $t$ for each node in the network.

We can think of $h(t)$ as the \textit{action},  which can be parameterized arbitrarily. The \textit{state} can be thought of as the time point $t$.

Policy-based methods can deal with large action spaces, including continuous action spaces. One way to do this is to assume that actions are chosen from a Gaussian distribution, which is defined by two parameters $\mu$ and $\sigma$ \cite{sutton2018reinforcement}. Let $\mu(t,W)=h(t)$, and let's chose $\sigma$ to be fixed.

To produce a policy parameterization,  the policy $\pi(z)$  can be defined by a Gaussian probability density function \cite{sutton2018reinforcement}. We have defined the \textit{mean} $\mu(t,W)$ to be given by a parametric function approximator that depends on the state $t$ and parameter $W$.


\begin{equation}
    \pi(z|t,W)  = \frac{1}{\sqrt{2 \pi} \sigma} e^{-(z-\mu(t,W))^2/(2\sigma^2)}
\end{equation}
The \textbf{policy gradient theorem} allows us to change the policy parameter in a way that ensures improvement by some scalar performance measure. The REINFORCE algorithm is based on the policy gradient theorem, and updates the parameters (in this case $W$ at time $t$) only based on the \textit{action} taken at time $t$ \cite{sutton2018reinforcement}. Using REINFORCE, we want to update our weights $W$ in the following manner:

\begin{equation}
\Delta W \propto \nabla \ln(\pi(z|t,W))
\end{equation}
where the policy $\pi(z|t,W)$ is a scalar performance measure and $\nabla \ln(\pi(z|t,W))$ is a stochastic estimate whose expectation approximates the gradient of the performance measure with respect to $W$ \cite{sutton2018reinforcement}.



In order to ensure exploration, some noise is added to the action such that the policy is not deterministic. Adding some noise to the hidden/recurrent layer:

$$
z = \mu(t,W) + \sigma \xi
$$
We can now calculate the main term for our update rule:

\begin{equation}
\nabla_W \ln(\pi(z|t,W)) = -\frac{1}{2 \sigma^2}\frac{\partial}{\partial W}\big(z - \mu(t,W)\big)^2 \\
=  -\frac{1}{2 \sigma^2}\frac{\partial}{\partial W}\big(z - (1-\frac{1}{\tau}) h(t-1) + \frac{1}{\tau}\phi(W \cdot h(t-1)) \big)^2
\end{equation}
Using the chain rule, we end up with:

\begin{equation}
= -\frac{1}{2 \sigma^2} 2 \Big(z - (1-\frac{1}{\tau}) h(t-1) - \frac{1}{\tau}\phi(W \cdot h(t-1)) \Big) \frac{\partial}{\partial W} \Big( (1-\frac{1}{\tau}) h(t-1) - \frac{1}{\tau}\phi(W \cdot h(t-1)) \Big)
\end{equation}
Substituting $z-\mu(t,W)=\sigma \xi$, we get:

\begin{equation}
= -\frac{1}{\sigma^2}  (\sigma \xi ) \frac{\partial}{\partial W} \Big( (1-\frac{1}{\tau}) h(t-1) - \frac{1}{\tau}\phi(W \cdot h(t-1)) \Big)
\end{equation}

\begin{equation}
= -\frac{1}{\sigma} \xi \Big( (1-\frac{1}{\tau}) \frac{\partial h(t-1) }{\partial W} - \frac{1}{\tau} \frac{\partial}{\partial W} \Big[  \phi(W \cdot h(t-1)) \Big] \Big)
\end{equation}
Looking at the right-most term and switching to summation notation:

\begin{equation}
    \frac{\partial}{\partial W_{ab}}\Big[  \phi \Big(\sum_j W_{ij} \cdot h_j(t-1) \Big) \Big] = \phi' \Big(\sum_j W_{ij} \cdot h_j(t-1) \Big) \sum_j W_{ij} \cdot   \frac{\partial h_j(t-1)}{\partial W_{ab}}
\end{equation}

\section{Enforcing Local Update (à la RFLO)}
The term $ \frac{\partial h_j(t-1)}{\partial W_{ab}} $ includes both \textit{local} and \textit{nonlocal} terms \cite{murray2019local}. However, we can separate the local and nonlocal weights and take the derivative:

\begin{equation}
    = \frac{\partial}{\partial W_{ab}}\Big[  \phi \Big(\sum_j W_{ij} \cdot h_j(t-1) \Big)_{i \neq a} + \delta_{ia} \phi \Big(\sum_j W_{aj} \cdot h_j(t-1) \Big) \Big]
\end{equation}
The derivative of the local term is simply:

\begin{equation}
    \frac{\partial}{\partial W_{ab}}\Big[ \delta_{ia} \phi \Big(\sum_j W_{ij} \cdot h_j(t-1) \Big) \Big] = \delta_{ia} \phi' \Big(\sum_j W_{ij} \cdot h_j(t-1) \Big) h_b(t-1)
\end{equation}
Keeping only the expression for the local terms, we arrive at:

\begin{equation}
    \frac{\partial}{\partial W_{ab}} \ln(\pi(z_i|t,W)) = -\frac{1}{\sigma} \xi \Bigg( (1-\frac{1}{\tau}) \frac{\partial h_i(t-1) }{\partial W_{ab}} - \frac{1}{\tau}  \delta_{ia} \phi' \Big(\sum_j W_{ij} \cdot h_j(t-1) \Big) h_b(t-1) \Bigg)
\end{equation}
Following RFLO, the recursive relation of the derivative with only \textit{local} terms is:

\begin{equation}
    \frac{\partial h_i(t-1) }{\partial W_{ab}}  = (1-\frac{1}{\tau}) \frac{\partial h_i(t-2) }{\partial W_{ab}} - \frac{1}{\tau}  \delta_{ia} \phi' \Big(\sum_j W_{ij} \cdot h_j(t-2) \Big) h_b(t-2)
\end{equation}
This therefore defines the eligibility trace. These two coupled, recursive equations can be expressed as:

\begin{equation}
    \overline{ \nabla_{W} \ln(\pi(z|t,W))}
\end{equation}
Finally, we can construct an update rule:

\begin{equation}
    \Delta W_{ij} = \eta (R^t - \bar{R}^t) \overline{ \nabla_{W} \ln(\pi(z|t,W))}
\end{equation}
where $(R^t - \bar{R}^t)$ is the return.

where $R^T$ is:

\begin{equation}
R^t = -(\text{distance to target})^2 - \lambda |\textbf{y}^t|^2
\end{equation}•

\begin{equation}
    W_{ij}(t+1) =  W{ij}(t) + \Delta W_{ij}
\end{equation}
