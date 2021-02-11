---
layout: default
---


There are many ways to implement reinforcement learning (RL) in recurrent neural networks (RNNs). A classic policy-gradient based method is REINFORCE [Williams 1992]; a more recent, more "biologically plausible" version is expounded in the paper **Biologically plausible learning in recurrent neural networks reproduces neural dynamics observed during cognitive tasks** [Miconi 2017]. The general rule is essentially Hebbian plasticity modulated by reward prediction error (RPE), and looks something like this:

$$ \Delta W_{ij} = \eta (R-\bar{R}) \bar{e}_{ij}$$

where recurrent RNN weights are updated by multiplying the deviation of reward $R$ from the _expected_ reward $\bar{R}$ (i.e. the RPE) and the eligibility trace $\bar{E}_{ij}$. So what is this eligibility trace?

What's neat is that "every neuron from neuron j to neuron i accumulates a _potential_ Hebbian weight change." Weights are only updated at the _end_ of the trial, depending on the RPE:

$$ \bar{e}_{ij}(t)_ =  \bar{e}_{ij}(t-1) + \phi( r_j(t-1) \cdot ((x_i(t) - \bar{x}_i))  )$$

This is essentially saying eligibility trace = output of neuron j $\times$ output of neuron i, which is Hebbian. Some extra details include a nonlinear function $\phi$, and a running average of recent activity $\bar{x}_i$ from neuron i (which has the effect of tracking short term/recent fluctuations).

There are a few nice things about this reinforcement learning (RL) weight update: one is that rewards only need to be given at the _end_ of the trial, unlike supervised learning (SL) where an error signal needs to be fed to the network at every time step.


How might we derive this learning rule for RNNs from "first principles?" One way to do this is to start with the policy-gradient theorem, and _remove_ nonlocal interaction terms in the RNN update (local meaning that a neuron can only receive signals/information from other neurons it is connected to). I go into more detail below:


## Policy Parameterization for Continuous Actions

Our general recurrent network (discretized) equation, without input, is:
\begin{equation}
    h(t) = (1-\frac{1}{\tau}) h(t-1) + \frac{1}{\tau}\phi(W \cdot h(t-1))
\end{equation}
$W$ is the recurrent connectivity matrix, and the vector $h(t)$ is the activity at time $t$ for each node in the network.

We can think of $h(t)$ as the _action_,  which can be parameterized arbitrarily. The _state_ can be thought of as the time point $t$.

Policy-based methods can deal with large action spaces, including continuous action spaces. One way to do this is to assume that actions are chosen from a Gaussian distribution, which is defined by two parameters $\mu$ and $\sigma$ [Sutton and Barto 2018]. Let $\mu(t,W)=h(t)$, and let's chose $\sigma$ to be fixed.

To produce a policy parameterization,  the policy $\pi(z)$  can be defined by a Gaussian probability density function [Sutton and Barto 2018]. We have defined the _mean_ $\mu(t,W)$ to be given by a parametric function approximator that depends on the state $t$ and parameter $W$.


\begin{equation}
    \pi(z|t,W)  = \frac{1}{\sqrt{2 \pi} \sigma} e^{-(z-\mu(t,W))^2/(2\sigma^2)}
\end{equation}

The _policy gradient theorem_ allows us to change the policy parameter in a way that ensures improvement by some scalar performance measure. The REINFORCE algorithm is based on the policy gradient theorem, and updates the parameters (in this case $W$ at time $t$) only based on the _action_ taken at time $t$ [Sutton and Barto 2018]. Using REINFORCE [Williams 1992], we want to update our weights $W$ in the following manner:

\begin{equation}
\Delta W \propto \nabla \ln(\pi(z|t,W))
\end{equation}
where the policy $\pi(z|t,W)$ is a scalar performance measure and $\nabla \ln(\pi(z|t,W))$ is a stochastic estimate whose expectation approximates the gradient of the performance measure with respect to $W$ [Sutton and Barto 2018].



In order to ensure exploration, some noise is added to the action such that the policy is not deterministic. Adding some noise to the hidden/recurrent layer:

$$ z = \mu(t,W) + \sigma \xi $$

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

## Enforcing Local Update


The term $ \frac{\partial h_j(t-1)}{\partial W_{ab}} $ includes both _local_ and _nonlocal_ terms [Murray 2019]. If we are interested in a biologically plausible learning rule, we only want _local_ terms. However, we can separate the local and nonlocal weights and take the derivative:

$$= \frac{\partial}{\partial W_{ab}}\Big[  \phi \Big(\sum_j W_{ij} \cdot h_j(t-1) \Big)^{i\neq a} + \delta_{ia} \phi \Big(\sum_j W_{aj} \cdot h_j(t-1) \Big) \Big]$$

The derivative of the local term is simply:

\begin{equation}
    \frac{\partial}{\partial W_{ab}}\Big[ \delta_{ia} \phi \Big(\sum_j W_{ij} \cdot h_j(t-1) \Big) \Big] = \delta_{ia} \phi' \Big(\sum_j W_{ij} \cdot h_j(t-1) \Big) h_b(t-1)
\end{equation}

Keeping only the expression for the local terms, we arrive at:

\begin{equation}
    \frac{\partial}{\partial W_{ab}} \ln(\pi(z_i|t,W)) = -\frac{1}{\sigma} \xi \Bigg( (1-\frac{1}{\tau}) \frac{\partial h_i(t-1) }{\partial W_{ab}} - \frac{1}{\tau}  \delta_{ia} \phi' \Big(\sum_j W_{ij} \cdot h_j(t-1) \Big) h_b(t-1) \Bigg)
\end{equation}

Following RFLO, the recursive relation of the derivative with only _local_ terms is:

\begin{equation}
    \frac{\partial h_i(t-1) }{\partial W_{ab}}  = (1-\frac{1}{\tau}) \frac{\partial h_i(t-2) }{\partial W_{ab}} - \frac{1}{\tau}  \delta_{ia} \phi' \Big(\sum_j W_{ij} \cdot h_j(t-2) \Big) h_b(t-2)
\end{equation}

This therefore defines the eligibility trace! While this eligibility trace is not exactly the same as the Miconi 2017 rule above, it essentially contains a postsynaptic term $\phi'(...)$ multiplied by a presynaptic term $h_b(t)$, with exploratory noise $\xi$

These two coupled, recursive equations can be expressed as:

\begin{equation}
    e_{ij}(t) = \overline{ \nabla_{W} \ln(\pi(z|t,W))}
\end{equation}

Finally, we can construct an update rule:

\begin{equation}
    \Delta W_{ij} = \eta (R(t) - \bar{R}(t)) e_{ij}
\end{equation}
where $(R(t) - \bar{R}(t))$ is the "return," and reward $R(t)$ is defined at the end of the trial as:

\begin{equation}
R(t) = -(\text{distance to target at end of trial})^2 - \lambda |\textbf{y}^t|^2
\end{equation}•

\begin{equation}
    W_{ij}(t+1) =  W{ij}(t) + \Delta W_{ij}
\end{equation}






## References

1. **Reinforcement Learning** Sutton and Barto, 2018
2. **Biologically plausible learning in recurrent neural networks reproduces neural dynamics observed during cognitive tasks** Miconi 2017
3. **Local online learning in recurrent networks with random feedback (RFLO)** Murray 2019


		      ________
		    .´\  ____ \
		 .´\ \´\_______\
		\´\´\´\/|__|__|_\
		 \´\´\/__|__|__|_\
		  \´\/_|__|__|__|_\
		   \/|__|__|__|__|_\


		    .─────────.
		 .´\ \ ·=====· \
		\´\´\´\_________\
		 \´\´\/__|__|__|_\
		  \´\/_|__|__|__|_\
		   \/|__|__|__|__|_\


		   ____________
		 .´\  ________ \
		\´\´\ \______´\ \
		 \´\´\___________\
		  \´\/_|__|__|__|_\
		   \/|__|__|__|__|_\


		 .─────────────.
		\´\ .─────────, \
		 \´\ \_______´_\ \
		  \´\_____________\
		   \/|__|__|__|__|_\

[ascii art](https://stonestoryrpg.com/ascii_tutorial.html)
