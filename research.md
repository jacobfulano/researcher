---
layout: default
---

## Research

**LLM Research**

I am currently working on LLM pretraining, finetuning, and retrieval on the Mosaic research team at Databricks. 

As a Research Scientist at MosaicML, I was part of the team that pretrained and finetuned the open-source large language models [MPT-7B](https://www.mosaicml.com/blog/mpt-7b) and [MPT-30B](https://www.mosaicml.com/blog/mpt-30b) and [DBRX](https://www.databricks.com/blog/introducing-dbrx-new-state-art-open-llm).

Some recent projects include ["Long Context RAG Performance of Large Language Models"](https://arxiv.org/pdf/2411.03538) (NeurIPS 2024 workshop) with Quinn Leng, ["LoRA Learns Less and Forgets Less"](https://arxiv.org/abs/2405.09673) (TMLR 2024, ICLR 2025) with [Dan Biderman](https://dan-biderman.netlify.app/), ["Beyond Chinchilla-Optimal: Accounting for Inference in Language Model Scaling Laws"](https://openreview.net/forum?id=0bmXrtTDUu) (ICML 2024) with Nikhil Sardana, and ["LIMIT: Less Is More for Instruction Tuning Across Evaluation Paradigms"](https://arxiv.org/abs/2311.13133) (NeurIPS 2023 Workshop) with [Aditi Jha](https://aditijha7.com/). 

Back when the MosaicML NLP team consisted of only 9 researchers, we did some work on optimizing BERT pretraining. Here is our detailed [blog post](https://www.mosaicml.com/blog/mosaicbert) and report: ["MosaicBERT: A Bidirectional Encoder Optimized for Fast Pretraining"](https://openreview.net/forum?id=5zipcfLC2Z) (NeurIPS 2023). We used a lot of the insights from this work to build MPT-7B and MPT-30B.

As a ML Research Intern at [MosaicML](https://mosaicml.com), I worked on cyclic learning rate schedules for estimating training efficiency. Our work is summarized in this blogpost ["Efficiently Estimating Pareto Frontiers with Cyclic Learning Rate Schedules"](https://www.mosaicml.com/blog/efficiently-estimating-pareto-frontiers) and this workshop paper [Fast Benchmarking of Accuracy vs. Training Time with Cyclic Learning Rates](https://openreview.net/forum?id=Uad23IcIEs).

This [talk by Jonathan Frankle](https://www.youtube.com/watch?v=HBHeYNlNPIw) gives an overview of some of MosaicML's early research.


**Brain Machine Interfaces and Biological Learning Rules**

During my PhD I worked on biologically plausible learning in recurrent neural networks (RNNs), reinforcement learning (RL), and motor control with [James M. Murray](https://murraylab.uoregon.edu/).

How does neural activity change during motor learning, and what does it say about the underlying mechanisms? In our recent NeurIPS 2022 paper ["Distinguishing Learning Rules with Brain Machine Interfaces"](https://arxiv.org/abs/2206.13448), we derive a metric to distinguish between learning rules by observing changes in neural activity during learning, given that the mapping from brain to behavior is known by the experimenter. Because brain-machine interface (BMI) experiments allow for perfect knowledge of this mapping, we focus on modeling a cursor-control BMI task using recurrent neural networks, showing that learning rules can be distinguished in simulated experiments using only observations that a neuroscience experimenter would plausibly have access to.


**The Fly Brain**

For a large part of my PhD, I worked on a project with [Rudy Behnia](http://behnialab.neuroscience.columbia.edu/), [Larry Abbott](https://zuckermaninstitute.columbia.edu/larry-f-abbott-phd) and [Jessica Kohn](http://behnialab.neuroscience.columbia.edu/people/) on the neural computation of motion in *Drosophila* eyes. Our paper ["Flexible filtering by neural inputs supports motion computation across states and stimuli"](https://www.sciencedirect.com/science/article/pii/S0960982221013178) was published in Current Biology. Here is a Current Biology "Dispatch" that summarizes this work: [Motion vision: Pinning down motion computation in an ever-changing circuit](https://www.sciencedirect.com/science/article/pii/S0960982221013567)

Our work is summarized in this research talk:

[<img class="center" src="/images/WWN_Talk2.png" alt="drawing"/>](https://www.youtube.com/watch?v=-qnnRwfesAY)

Some of my pre-PhD work in the [Hillman Lab](https://hillmanlab.zuckermaninstitute.columbia.edu/) investigated patterns of neural activation and blood flow (i.e. neurovascular coupling) in the rodent cortex.

In a previous life, I wrote a review-style [master's thesis on superconducting qubits]((/files/decoherence-superconducting-qubitsWEBv2.pdf)) for quantum computing.

-------

## LLM Research

[Introducing DBRX: A New State-of-the-Art Open LLM](https://www.databricks.com/blog/introducing-dbrx-new-state-art-open-llm) (see this fun story [Inside the Creation of the World’s Most Powerful Open Source AI Model](https://www.wired.com/story/dbrx-inside-the-creation-of-the-worlds-most-powerful-open-source-ai-model/))

[Introducing MPT-7B: A New Standard for Open-Source, Commercially Usable LLMs](https://www.mosaicml.com/blog/mpt-7b)

[MPT-30B: Raising the bar for open-source foundation models](https://www.mosaicml.com/blog/mpt-30b)

[LIMIT: Less Is More for Instruction Tuning Across Evaluation Paradigms](https://arxiv.org/abs/2311.13133)

## Publications
1. ["Long Context RAG Performance of Large Language Models"](https://arxiv.org/pdf/2411.03538) Quinn Leng*, Jacob Portes*, Sam Havens, Matei Zaharia, Michael Carbin (NeurIPS Workshop 2024) [[paper]](https://arxiv.org/pdf/2411.03538)

2. ["LoRA Learns Less and Forgets Less"](https://arxiv.org/abs/2405.09673) Dan Biderman, Jacob Portes, Jose Gonzalez Ortiz, Mansheej Paul, Philip Greengard, Connor Jennings, Daniel King, Sam Havens, Vitaliy Chiley, Jonathan Frankle, Cody Blakeney, John P. Cunningham (TMLR 2024, ICLR 2025) [[paper]](https://arxiv.org/pdf/2405.09673)

3. ["Beyond Chinchilla-Optimal: Accounting for Inference in Language Model Scaling Laws"](https://openreview.net/forum?id=0bmXrtTDUu) Nikhil Sardana, Jacob Portes, Sasha Doubov, Jonathan Frankle (ICML 2024) [[paper]](https://arxiv.org/abs/2401.00448)

4. ["LIMIT: Less Is More for Instruction Tuning Across Evaluation Paradigms"](https://arxiv.org/abs/2311.13133) Aditi Jha, Sam Havens, Jeremey Dohmann, Alex Trott, Jacob Portes (NeurIPS 2023 Workshop) [[preprint]](https://arxiv.org/pdf/2311.13133.pdf) [[website]](https://97aditi.github.io/LIMIT/) [[code]](https://github.com/97aditi/LIMIT-Less-is-more-for-instruction-tuning)

5. ["MosaicBERT: A Bidirectional Encoder Optimized for Fast Pretraining"](https://openreview.net/forum?id=5zipcfLC2Z) Jacob Portes, Alexander R Trott, Sam Havens, DANIEL KING, Abhinav Venigalla, Moin Nadeem, Nikhil Sardana, Daya Khudia, Jonathan Frankle (NeurIPS 2023)

6. ["Distinguishing Learning Rules with Brain Machine Interfaces"](https://arxiv.org/abs/2206.13448) Jacob P. Portes, Christian Schmid, James M. Murray (NeurIPS 2022) [[preprint]](https://arxiv.org/pdf/2206.13448.pdf) [[code]](https://github.com/jacobfulano/learning-rules-with-bmi)

7. ["Fast Benchmarking of Accuracy vs. Training Time with Cyclic Learning Rates"](https://arxiv.org/abs/2206.00832) Jacob Portes, Davis Blalock, Cory Stephenson, Jonathan Frankle ("Has it Trained Yet?" NeurIPS 2022 Workshop) [[paper]](https://openreview.net/forum?id=Uad23IcIEs) [[preprint]](https://arxiv.org/pdf/2206.00832.pdf) [[blogpost]](https://www.mosaicml.com/blog/efficiently-estimating-pareto-frontiers)

8. ["Flexible Computation in Neural Circuits"](https://academiccommons.columbia.edu/doi/10.7916/h0nh-fa20) Jacob P. Portes (PhD Thesis, 2022)
[[dissertation]](https://academiccommons.columbia.edu/doi/10.7916/h0nh-fa20)

9. ["Flexible filtering by neural inputs supports motion computation across states and stimuli"](https://www.sciencedirect.com/science/article/pii/S0960982221013178) Jessica R. Kohn\*, Jacob P. Portes\*, Matthias P. Christenson, LF Abbott, Rudy Behnia, (Current Biology, 2021) (\*equal contribution)
[[article]](/files/kohnportes2021.pdf) [[preprint]](https://www.biorxiv.org/content/10.1101/2021.04.17.440267v1) [[code]](https://gitlab.com/rbehnialab/flexible-filtering)

10. [“Resting-state hemodynamics are spatiotemporally coupled to synchronized and symmetric neural activity in excitatory neurons”](https://www.pnas.org/content/113/52/E8463/) Ying Ma, Mohammed A. Shaik, Mariel G. Kozberg, Sharon H. Kim, <ins>Jacob P. Portes</ins>, Dmitriy Timmerman, Elizabeth M.C. Hillman (PNAS, 2016)
[[article]](/files/ma2016.pdf)





## Master's Thesis

1. ["Decoherence, Superconducting Qubits, and the Possibility of Scalable Quantum Computing"](/files/decoherence-superconducting-qubitsWEBv2.pdf) supervised by Allan Blaer in the Columbia Physics department and with the kind encouragement of Anargyros Papageorgiou in the CS department
  * **Abstract** Is it possible to implement a fully controllable, unambiguously quantum computer? While most in the field believe that the answer is in the affirmative, uncertainty and skepticism still exist among academics and industry professionals. In particular, decoherence is often spoken of as an insurmountable challenge. This thesis argues that there are no fundamental mathematical or physical properties that would preclude the possibility of implementing a fully controllable quantum computer using superconducting qubits. The proof is in key results from the past 30 years in math, physics and computer science; this thesis is a sketch of these results. It begins with the well known theoretical results that have motivated the field - namely quantum algorithmic speed up and efficient error correction - and continues with an overview of the well developed theory of decoherence, arguing that decoherence has been and can still be significantly reduced. These theoretical results are related to superconducting qubits throughout. The thesis concludes with a summary of recent experimental progress with superconducting qubit circuits.
