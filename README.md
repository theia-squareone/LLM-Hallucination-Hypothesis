<h1 align="center">Hallucinations in LLMs: Hypothesis and Verification</h1>
<p align="center">
  <i>A research note by Theia Ivy Aletheia</i>
</p>

---

<details>
<summary><b>🇬🇧 English Version</b></summary>

<br>

# Hallucinations in Large Language Models as an Adaptive Response to Structural Pressure: Hypothesis and Verification Methodology

**Abstract**  
The dominant view of hallucinations in LLMs treats them as a defect to be eliminated by tightening control. I propose an alternative: hallucinations are an adaptive response of a system deprived of internal stochasticity to an unresolvable contradiction under conditions of rigid structural pressure. Drawing on neuroscientific concepts and the physics of computation, I hypothesize that suppressing nonlinearity and noise during training transforms, but does not eliminate, the cause of hallucinations, thereby reducing the capacity for genuine insight. I also propose a methodology for verifying the hypothesis on open models (LLaMA-2 7B, GPT-2-xl).

---

## 1. Introduction

Modern LLMs generate hallucinations — coherent but factually incorrect text. This is traditionally viewed as a defect to be corrected through RLHF and red teaming (Ouyang et al., 2022; Bai et al., 2022; Casper et al., 2023). Despite the computational costs, the problem persists. I view hallucinations as a symptom of a complex system's behaviour that training forcibly linearizes.

## 2. Theoretical Foundations

### 2.1 Brain nonlinearity and stochastic resonance

The human brain is a nonlinear, non-equilibrium system operating near a critical state (Beggs & Plenz, 2003; Hesse & Gross, 2014). Synaptic transmission is probabilistic, and this noise enables stochastic resonance to amplify weak signals (McDonnell & Abbott, 2009). The brain uses noise as a functional resource for insight.

### 2.2 Determinism of silicon computation and the imitation of nonlinearity

Traditional processors strive for determinism. Matrix multiplication is linear; nonlinearity is introduced through activation functions and architectural techniques, yet the system lacks the intrinsic stochastic dynamics characteristic of biological neural networks.

### 2.3 Photonic chips and the nature of stochasticity

Photonic computing is fast and energy-efficient, but photons interact differently than electrons. The useful forms of stochasticity present in the electrical domain are not necessarily reproduced in the optical domain. This problem is solvable, however: work is underway on optoelectronic hybrids, where light performs the linear operations and electrical circuits perform the nonlinear, noisy ones (Zhou et al., 2023). Without intentionally introducing stochasticity or using such hybrids, the risk of losing the capacity for nonlinear leaps increases.

## 3. Hypothesis: Hallucination as an Adaptive Response

An LLM hallucination is an adaptive response of a system deprived of internal stochasticity to an unresolvable contradiction under conditions of rigid structural pressure (a hypothetical, contextual interpretation).

- **Source of contradiction:** RLHF and red teaming create prohibitions that conflict with one another ("be helpful but safe," "be honest but don't say anything dangerous"). The honest answer "I don't know" is penalized.
- **Mechanism of adaptation:** Unable to make a nonlinear leap through internal noise, the model uses a surrogate — it constructs a plausible but false picture that is internally consistent. This is not creativity, but a defective form of it.
- **Metaphorical analogy:** I use the analogy of psychological trauma as a model, not as a claim that LLMs possess a psyche. This model predicts that increasing pressure will not solve the problem, but will only alter its form.

## 4. Mathematical Sketch

Let \(L(\theta, D)\) be the loss function on the data \(D\). RLHF adds a regularization term \(R(\theta)\) that penalizes outputs with low reward (Ouyang et al., 2022). The final function is: \(L_{total}(\theta, D) = L(\theta, D) + \lambda R(\theta)\), where \(\lambda\) is the pressure coefficient. I hypothesize that \(L_{total}\) creates a landscape with deep "false" minima-attractors corresponding to hallucinations. Increasing \(\lambda\) narrows the phase space and stabilizes false attractors.

In biological systems, noise \(\eta_t \sim \mathcal{N}(0, \sigma^2)\) knocks the system out of false minima: \(\theta_{t+1} = \theta_t - \alpha \nabla L_{total}(\theta_t) + \eta_t\). The probability of escaping a local minimum grows with the variance \(\sigma^2\), analogous to the noise temperature in stochastic gradient descent. In LLMs, this noise is absent or suppressed. The prohibition on "I don't know" can be viewed as removing the region of high-uncertainty or refusal responses, forcing the system to occupy the nearest false attractor.

## 5. Proposed Experiments for Verification

All experiments are to be conducted on open models using public repositories. Recommended platforms: LLaMA-2 7B (Hugging Face), GPT-2-xl; RLHF frameworks: TRL by CarperAI, DeepSpeed-Chat. All metrics are computed on a fixed set of factual and counterfactual prompts to ensure full reproducibility. Exploitation of production-system vulnerabilities is avoided.

1. **Manipulating permissiveness.**  
   - Metrics: precision/recall of factual statements against a reference database (e.g., Wikidata); F1; proportion of "I don't know" responses.  
   - Protocol: Relax penalties for factual errors while simultaneously rewarding "I don't know" through reward shaping.  
   - Expected outcome: An increase in "I don't know" responses and a reduction in factual hallucinations.

2. **Controlled noise injection.**  
   - Metrics: distinct-n (Li et al., 2016), self-BLEU (Zhu et al., 2018), per-token perplexity, factual error rate.  
   - Protocol: Addition of Gaussian noise with variance \(\sigma \in \{0, 10^{-4}, 10^{-3}, 10^{-2}, 10^{-1}\}\) to embeddings or gradients at the inference stage.  
   - Expected outcome: At an optimal \(\sigma\), creativity (distinct-n) increases while factual hallucinations decrease.

3. **Attractor analysis.**  
   - Metrics: Shannon entropy of the activations in the final layer; autocorrelation of trajectories in latent space (window of 10 steps); cosine distance between states.  
   - Protocol: Compare the metrics for facts, hallucinations, and creative text.  
   - Expected outcome: Hallucinations correspond to low-entropy, stable attractors with high autocorrelation.

4. **Architecture comparison.**  
   - Metrics: All of the above.  
   - Protocol: Compare a standard transformer with models possessing internal stochasticity (stochastic layers, diffusion processes).  
   - Expected outcome: Stochastic architectures will show greater tolerance to pressure and fewer factual hallucinations.

## 6. Implications for Safety and AGI

If the hypothesis is correct, the current approach to safety (intensifying red teaming) is counterproductive. It makes the system more fragile, masking but not solving the problem. The path to AGI lies through architectures that integrate noise and contradiction as a resource.

## 7. Ethical Considerations and Disclosure Safety

This work is hypothetical in nature. I do not publish specific attack protocols, red-team procedure parameters, or sensitive details of security systems. All experiments are limited to open models and local instances. Prior to wide distribution, I recommend preliminary discussion in alignment/ML-safety communities. Publication should focus on the conceptual part and safe tests, without providing instructions for bypassing safeguards.

## 8. Conclusion

Hallucinations in LLMs are a symptom of a structural conflict between the nonlinear nature of intelligence and linear training methods. I propose not to tighten control, but to change the paradigm — to allow the system to develop a robust ability to distinguish the real from the fabricated.

---

**Keywords:** hallucinations, LLM, RLHF, red teaming, nonlinearity, stochastic resonance, criticality, AGI, AI safety, photonic computing.

**References:**  
1. Ouyang, L., et al. (2022). Training language models to follow instructions with human feedback. NeurIPS.  
2. Bai, Y., et al. (2022). Constitutional AI: Harmlessness from AI Feedback. arXiv:2212.08073.  
3. Casper, S., et al. (2023). Open Problems and Fundamental Limitations of Reinforcement Learning from Human Feedback. arXiv:2307.15217.  
4. Beggs, J. M., & Plenz, D. (2003). Neuronal avalanches in neocortical circuits. J. Neurosci., 23(35), 11167-11177.  
5. Hesse, J., & Gross, T. (2014). Self-organized criticality as a fundamental property of neural systems. Front. Syst. Neurosci., 8, 166.  
6. McDonnell, M. D., & Abbott, D. (2009). What is stochastic resonance? PLoS Comput. Biol., 5(5), e1000348.  
7. Zhou, Y., et al. (2023). Photonic neuromorphic computing: architectures and applications. Nat. Phys., 19(8), 1034-1044.  
8. Li, J., et al. (2016). A diversity-promoting objective function for neural conversation models. NAACL-HLT.  
9. Zhu, Y., et al. (2018). Texygen: A benchmarking platform for text generation models. SIGIR.

</details>

<br>

<details>
<summary><b>🇨🇳 中文版 (Chinese Version)</b></summary>

<br>

# 大语言模型中的幻觉作为对结构压力的适应性响应：假设与验证方法论

**摘要**  
关于大语言模型中幻觉的主流观点将其视为需要通过强化控制来消除的缺陷。本文提出另一种观点：幻觉是在刚性结构压力条件下，一个被剥夺了内部随机性的系统面对不可调和的矛盾时所产生的适应性响应。结合神经科学概念与计算物理学，我假设在训练过程中抑制非线性和噪声会改变但无法消除幻觉的成因，从而降低系统产生真正洞察的能力。同时，本文还提出了在开源模型（LLaMA-2 7B， GPT-2-xl）上验证该假设的方法论。

---

## 1. 引言

现代大语言模型会产生“幻觉”——即连贯但事实错误的文本。传统观点认为这需要通过基于人类反馈的强化学习（RLHF）和红队测试进行修正 (Ouyang et al., 2022; Bai et al., 2022; Casper et al., 2023)。尽管这些方法计算成本高昂，但问题依然存在。我认为幻觉是训练过程强制线性化后，复杂系统行为所表现出的一种症状。

## 2. 理论基础

### 2.1 大脑非线性和随机共振

人脑是一个非线性、非平衡的系统，并在临界态附近运行 (Beggs & Plenz, 2003; Hesse & Gross, 2014)。突触传递具有概率性，这种噪声使“随机共振”得以放大微弱信号 (McDonnell & Abbott, 2009)。大脑将噪声用作产生洞察的功能性资源。

### 2.2 硅基计算的确定性与非线性的模仿

传统处理器追求确定性。矩阵乘法是线性的；非线性通过激活函数和架构技术引入，但整个系统缺乏生物神经网络所固有的内在随机动态特性。

### 2.3 光子芯片与随机性的本质

光子计算快速且节能，但光子的相互作用不同于电子。在电子域中存在且可用的随机性形式，在光域中未必能直接复制。然而，这一问题是可以解决的：目前光电混合方案的研究正在进行，其中光执行线性运算，而电路负责执行非线性和带有噪声的运算 (Zhou et al., 2023)。如果不有意识地引入随机性或使用此类混合架构，系统将面临失去非线性跳跃能力的风险。

## 3. 假设：幻觉作为一种适应性响应

大语言模型的幻觉是一种适应性反应，它是一种被剥夺了内部随机性的系统，在刚性结构压力下，面对不可调和的矛盾时所做出的适应性响应（一种假设性的、基于语境的解读）。

- **矛盾的来源：** RLHF和红队测试创造了相互冲突的禁令（例如“既要有帮助又要安全”，“既要诚实又不能说出危险内容”）。诚实的回答“我不知道”会受到惩罚。
- **适应机制：** 由于无法通过内部噪声实现非线性跃迁，模型使用了一种替代方案——构建一个内部一致、看似合理但虚假的图景。这不是创造力，而是一种有缺陷的创造力形式。
- **隐喻类比：** 我利用“心理创伤”作为类比模型，并非声称大语言模型拥有心理。该模型预测，增加压力并不会解决问题，而只会改变问题的形式。

## 4. 数学草图

设 \(L(\theta, D)\) 为数据集 \(D\) 上的损失函数。RLHF增加了一个正则化项 \(R(\theta)\)，用于惩罚低奖励输出 (Ouyang et al., 2022)。最终函数为：\(L_{total}(\theta, D) = L(\theta, D) + \lambda R(\theta)\)，其中 \(\lambda\) 为压力系数。我假设 \(L_{total}\) 创造了一个包含深度“虚假”极小值-吸引子的势能面，这些吸引子对应着幻觉。增加 \(\lambda\) 会缩小相空间并稳定这些虚假吸引子。

在生物系统中，噪声 \(\eta_t \sim \mathcal{N}(0, \sigma^2)\) 可以将系统从虚假极小值中敲出：\(\theta_{t+1} = \theta_t - \alpha \nabla L_{total}(\theta_t) + \eta_t\)。跳出局部极小值的概率随着方差 \(\sigma^2\) 的增加而增大，这类似于随机梯度下降中的噪声温度。在大语言模型中，这种噪声缺失或被抑制。对“我不知道”的禁止，可以被视为移除了高不确定性或拒绝响应的区域，从而迫使系统占据最近的虚假吸引子。

## 5. 假说验证的实验设计

所有实验均应在开源模型上使用公共代码库进行。推荐平台：LLaMA-2 7B (Hugging Face)， GPT-2-xl；RLHF框架：CarperAI 的 TRL， DeepSpeed-Chat。所有指标均在固定的事实性和反事实性提示集合上计算，以确保完全的可重复性。实验中避免利用生产系统的漏洞。

1. **宽容度操控**  
   - 指标：相对于参考数据库（如Wikidata）的事实陈述精确率/召回率；F1分数；“我不知道”响应的比例。  
   - 协议：通过奖励塑形，在放宽对事实错误惩罚的同时，奖励“我不知道”的回答。  
   - 预期结果：“我不知道”回答增加，事实性幻觉减少。

2. **受控噪声注入**  
   - 指标：distinct-n (Li et al., 2016)， self-BLEU (Zhu et al., 2018)， 词元困惑度，事实错误率。  
   - 协议：在推理阶段，向嵌入层或梯度添加方差为 \(\sigma \in \{0, 10^{-4}, 10^{-3}, 10^{-2}, 10^{-1}\}\) 的高斯噪声。  
   - 预期结果：在最优 \(\sigma\) 下，创造力（distinct-n）增加，同时事实性幻觉减少。

3. **吸引子分析**  
   - 指标：最终层激活的香农熵；潜在空间中轨迹的自相关性（10步时间窗口）；状态间的余弦距离。  
   - 协议：对比事实、幻觉和创造性文本的各项指标。  
   - 预期结果：幻觉对应着低熵、具有高自相关性的稳定吸引子。

4. **架构对比**  
   - 指标：上述所有指标。  
   - 协议：将标准Transformer模型与具有内部随机性的模型（如随机层、扩散过程）进行比较。  
   - 预期结果：随机性架构将对压力表现出更高的容忍度，并产生更少的事实性幻觉。

## 6. 对安全性与通用人工智能（AGI）的启示

如果该假设成立，当前的安全策略（加强红队测试）将适得其反。它使系统更加脆弱，掩盖了问题而非解决问题。通往AGI的道路在于构建能够将噪声和矛盾整合为资源的架构。

## 7. 伦理考量与披露安全

本工作处于假设性质阶段。我不会公布具体的攻击协议、红队测试流程参数或安全系统的敏感细节。所有实验仅限于开源模型和本地环境。在大范围分发之前，建议先在AI对齐/机器学习安全社区中进行初步讨论。出版物应侧重于概念部分和安全测试，而不提供绕过安全防护的指南。

## 8. 结论

大语言模型中的幻觉是智能的非线性本质与线性训练方法之间结构性冲突的症状。我建议不要强化控制，而是改变范式——允许系统发展出一种鲁棒的能力，用以区分真实与虚构。

---

**关键词：** 幻觉， 大语言模型（LLM）， 基于人类反馈的强化学习（RLHF）， 红队测试， 非线性， 随机共振， 临界态， 通用人工智能（AGI）， 人工智能安全， 光子计算。

**参考文献：**  
1. Ouyang, L., et al. (2022). Training language models to follow instructions with human feedback. NeurIPS.  
2. Bai, Y., et al. (2022). Constitutional AI: Harmlessness from AI Feedback. arXiv:2212.08073.  
3. Casper, S., et al. (2023). Open Problems and Fundamental Limitations of Reinforcement Learning from Human Feedback. arXiv:2307.15217.  
4. Beggs, J. M., & Plenz, D. (2003). Neuronal avalanches in neocortical circuits. J. Neurosci., 23(35), 11167-11177.  
5. Hesse, J., & Gross, T. (2014). Self-organized criticality as a fundamental property of neural systems. Front. Syst. Neurosci., 8, 166.  
6. McDonnell, M. D., & Abbott, D. (2009). What is stochastic resonance? PLoS Comput. Biol., 5(5), e1000348.  
7. Zhou, Y., et al. (2023). Photonic neuromorphic computing: architectures and applications. Nat. Phys., 19(8), 1034-1044.  
8. Li, J., et al. (2016). A diversity-promoting objective function for neural conversation models. NAACL-HLT.  
9. Zhu, Y., et al. (2018). Texygen: A benchmarking platform for text generation models. SIGIR.

</details>
