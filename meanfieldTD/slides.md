---
theme: ../common/themes/neat
routerMode: hash
layout: cover
colorSchema: light

coverTitle: |
  On the Convergence of Semi-Gradient TD:
  A Mean-Field Perspective

coverAuthor: Haruki Settai
coverCollaborator: "Tadashi Kozuno"
coverSupervisor: "Shinji Ito"

lineNumbers: true
---


---
pageNumber: true
---

<div style="font-size: 1.2em;">
  <Toc minDepth="1" maxDepth="2" />
</div>

---
layout: section
subject: Introduction
---

# Introduction

---
layout: default
headerEnable: true
pageNumber: true
---


<div class="slide-title-no-toc slide-title-h3" style="position: absolute; left: 40px; top: 65px;">
Introduction
</div>

<div class="intro-timeline">

  <div v-click class="intro-card card-td">
    <img src="./public/intro/TD-Gammon.png" class="intro-img contain" />
    <div class="intro-text">
      <div class="intro-year">1995</div>
      <div class="intro-title">TD-Gammon</div>
      <div class="intro-desc">A landmark success of TD learning.</div>
      <div class="intro-source">Figure from: Sutton &amp; Barto,<br>Reinforcement Learning, 1998.</div>
    </div>
  </div>

  <div v-click class="intro-card card-qt">
    <img src="./public/intro/qt-opt.png" class="intro-img cover" />
    <div class="intro-text">
      <div class="intro-year">2018</div>
      <div class="intro-title">QT-Opt</div>
      <div class="intro-desc">Large-scale Q-learning for robotic grasping.</div>
      <div class="intro-source">Figure from: Kalashnikov et al., 2018.</div>
    </div>
  </div>

  <div v-click class="intro-card card-dota">
    <img src="./public/intro/dota2.png" class="intro-img cover" />
    <div class="intro-text">
      <div class="intro-year">2019</div>
      <div class="intro-title">OpenAI Five</div>
      <div class="intro-desc">Large-scale RL for complex team games..</div>
      <div class="intro-source"><sup>©</sup> Valve Corporation.</div>
    </div>
  </div>

  <div v-click class="intro-card card-rlhf">
    <img src="./public/intro/RLHF.png" class="intro-img cover" />
    <div class="intro-text">
      <div class="intro-year">2022</div>
      <div class="intro-title">RLHF</div>
      <div class="intro-desc">RL from learned human preferences.</div>
      <div class="intro-source">Image from: OpenAI.</div>
    </div>
  </div>

  <div v-click class="intro-card card-dreamer">
    <img src="./public/intro/dreamer.png" class="intro-img cover" />
    <div class="intro-text">
      <div class="intro-year">2019–2025</div>
      <div class="intro-title">Dreamer V1–4</div>
      <div class="intro-desc">Learning world models for control.</div>
      <div class="intro-source">Figure from: Hafner et al., 2023.</div>
    </div>
  </div>

</div>

<div v-click class="intro-td-text">
  <div class="intro-sentence">
    These successes of Deep RL are based on <strong class="accent">TD learning</strong>. However, the convergence of TD learning with function approximation remains poorly understood.
  </div>
</div>

<div v-click>
  <div class="intro-posttraining-text-1">
    Recent successes in LLM post-training are often not based on TD learning, as they can use outcome-based feedback after generating complete answers.
  </div>

  <div class="intro-llm-area">
    <div class="intro-llm-line"></div>
    <div class="intro-llm-card">
      <img src="./public/intro/GRPO.png" class="intro-img contain" />
      <div class="intro-text">
        <div class="intro-year">recent</div>
        <div class="intro-title">LLM post-training</div>
        <div class="intro-desc">GRPO, DPO, etc.</div>
        <div class="intro-source">Figure from: DeepSeek-AI, 2025.</div>
      </div>
    </div>
  </div>
</div>

<div v-click class="intro-posttraining-text-2">
  Still, in continuing or long-horizon tasks, such full feedback is not always available, TD learning remains a fundamental component for deep RL.
</div>

<div v-click class="intro-question">
  <span class="question-label">Our question:</span>
  Can TD learning converge with neural-network approximation?
</div>

<style>
.intro-timeline {
  position: absolute;
  left: 48px;
  top: 8px;
  width: 460px;
  height: 420px;
}

.intro-card {
  position: absolute;
  left: 0;
  display: flex;
  align-items: center;
  gap: 12px;
  width: 430px;
  height: 60px;
  padding: 5px 8px;
  background: transparent;
}

.card-td {
  bottom: 0px;
}

.card-qt {
  bottom: 68px;
}

.card-dota {
  bottom: 136px;
}

.card-rlhf {
  bottom: 204px;
}

.card-dreamer {
  bottom: 272px;
}

.intro-img {
  width: 110px;
  height: 52px;
  border-radius: 7px;
  background: rgba(245, 245, 245, 0.85);
  flex-shrink: 0;
}

.intro-img.cover {
  object-fit: cover;
}

.intro-img.contain {
  object-fit: contain;
  padding: 2px;
}

.intro-text {
  flex: 1;
  min-width: 0;
}

.intro-year {
  font-size: 0.55rem;
  line-height: 1.0;
  font-weight: 700;
  color: rgba(34, 36, 72, 0.56);
  margin-bottom: 1px;
}

.intro-title {
  font-size: 0.78rem;
  line-height: 1.05;
  font-weight: 900;
  color: rgba(34, 36, 72, 0.96);
}

.intro-desc {
  margin-top: 4px;
  font-size: 0.55rem;
  line-height: 1.08;
  font-weight: 500;
  color: rgba(34, 36, 72, 0.72);
}

.intro-source {
  margin-top: 4px;
  font-size: 0.41rem;
  line-height: 1.05;
  font-weight: 400;
  color: rgba(34, 36, 72, 0.28);
}

.intro-td-text {
  position: absolute;
  left: 455px;
  top: 105px;
  width: 385px;
  font-size: 0.9rem;
  line-height: 1.36;
  color: rgba(34, 36, 72, 0.92);
}

.intro-posttraining-text-1 {
  position: absolute;
  left: 455px;
  top: 205px;
  width: 385px;
  font-size: 0.9rem;
  line-height: 1.36;
  font-weight: 500;
  color: rgba(34, 36, 72, 0.92);
}

.intro-posttraining-text-2 {
  position: absolute;
  left: 455px;
  top: 300px;
  width: 385px;
  font-size: 0.9rem;
  line-height: 1.36;
  font-weight: 500;
  color: rgba(34, 36, 72, 0.92);
}

.intro-sentence {
  margin-top: 6px;
  font-weight: 500;
}

.intro-llm-area {
  position: absolute;
  left: 48px;
  top: 444px;
  width: 360px;
  height: 86px;
}

.intro-llm-line {
  position: absolute;
  left: 0;
  top: 0;
  width: 310px;
  border-top: 1.4px dashed rgba(34, 36, 72, 0.34);
}

.intro-llm-card {
  position: absolute;
  left: 0;
  top: 12px;
  display: flex;
  align-items: center;
  gap: 12px;
  width: 430px;
  height: 60px;
  padding: 5px 8px;
  background: transparent;
}

.intro-question {
  position: absolute;
  left: 455px;
  top: 420px;
  width: 385px;
  padding: 12px 16px;
  border: 1.6px solid rgba(45, 45, 45, 0.65);
  border-radius: 14px;
  font-size: 0.9rem;
  line-height: 1.3;
  font-weight: 700;
  background: rgba(255, 255, 255, 0.82);
}

.question-label {
  color: #C00000;
  font-weight: 900;
  margin-right: 8px;
}

.slide-title-h3 {
  font-size: 1.13rem;
  line-height: 1.2;
  font-weight: 900;
}
</style>

---
layout: section
subject: Background
---

# Background

---
layout: default
headerEnable: true
headerTitle: Background
pageNumber: true
---

## Reinforcement Learning

The goal of Reinforcement Learning (RL) is to learn a good policy in sequential decision making.

<div v-click style="position: absolute; left: 290px; top: 150px; width: 70%; font-size: 0.92rem;">

$$\begin{alignedat}{4}&\text{State Space}\quad&\mathcal{S}&\subseteq\mathbb{R}^n,\;\text{bounded}\qquad&\text{Reward}\quad&r&:\mathcal{S}\times\mathcal{A}\to[0,1]\\ &\text{Action Space}\quad&\mathcal{A}&\subseteq\mathbb{R}^m,\;\text{bounded}\qquad&\text{Policy}\quad&\pi&:\mathcal{S}\to\mathcal{P}(\mathcal{A})\\ &\text{Transition Model}\quad&P&:\mathcal{S}\times\mathcal{A}\to\mathcal{P}(\mathcal{S})\qquad&&&\end{alignedat}$$

</div>

<div v-click.hide style="position: absolute; left: 290px; top: 270px; width: 70%; font-size: 0.92rem;">
<div v-click>

$$\begin{aligned}&\text{We are interested in total return:}\\ &\qquad \mathbb{E}_{\substack{A_t\sim\pi(\cdot\mid S_t)\\ S_{t+1}\sim P(\cdot\mid S_t,A_t)}}\left[\sum_{t=0}^\infty r(S_t,A_t)\bigg|S_0=s\right]\quad\text{for all }s\in\mathcal{S}\end{aligned}$$

</div>
</div>

<div v-after style="position: absolute; left: 290px; top: 270px; width: 70%; font-size: 0.92rem;">

$$\begin{aligned}&\text{Value function for }\pi\\ &\qquad V^\pi(s)=\mathbb{E}_{\substack{A_t\sim\pi(\cdot\mid S_t)\\ S_{t+1}\sim P(\cdot\mid S_t,A_t)}}\left[\sum_{t=0}^\infty{\color{red}\gamma^t}r(S_t,A_t)\bigg|S_0=s\right]\quad\gamma\in(0,1)\end{aligned}$$

</div>


<div v-click.hide style="position: absolute; left: 550px; top: 370px; width: 58%; font-size: 0.74rem; opacity: 0.78;">
<div v-click>

Without discounting, value may diverge, making it <br>
impossible to compare states:<br>
From $s_1$ : $+1+1+1+1+1+\cdots$ <br>
From $s_2$ : $+2+2+2+2+2+\cdots$

</div>
</div>

<div v-after style="position: absolute; left: 217px; top: 400px; width: 70%; font-size: 0.92rem;">

$$\begin{aligned}&\text{Bellman Equation}\\ &\qquad V^\pi(s)=\mathbb{E}_{\substack{A\sim\pi(\cdot\mid s)\\ S'\sim P(\cdot\mid s,A)}}\bigg[r(s,A)+\gamma V^\pi(S')\bigg]\end{aligned}$$

</div>



<SequentialDecisionFrames />


---
layout: default
headerEnable: true
headerTitle: Background
---

### Example: Computing the Value Function

<div style="font-size: 0.8em; position: absolute; top: 17%; left: 8% ;">

$$\begin{alignedat}{2}&\text{Definition}&&\text{Bellman Equation}\\ &\quad V^\pi(s)=\mathbb{E}_{\substack{A_t\sim\pi(\cdot\mid S_t)\\ S_{t+1}\sim P(\cdot\mid S_t,A_t)}}\left[\sum_{t=0}^\infty\gamma^tr(S_t,A_t)\bigg|S_0=s\right]\qquad&&\qquad V^\pi(s)=\mathbb{E}_{\substack{A\sim\pi(\cdot\mid s)\\ S'\sim P(\cdot\mid s,A)}}\bigg[r(s,A)+\gamma V^\pi(S')\bigg]\end{alignedat}$$

</div>


<div style="position: absolute; top: 10%; right: 4%; font-size: 0.72rem; line-height: 1.15; padding: 10px 10px 0px 10px; border: 1px solid rgba(120,120,120,0.28); border-radius: 12px; background: rgba(255,255,255,0.55); backdrop-filter: blur(6px);">

<div style="font-weight: 800; margin-bottom: 4px;">Grid-world setting</div>

<div><span style="display:inline-block; width: 10px; height: 10px; border: 2px solid rgba(60,150,255,0.95); border-radius: 3px; vertical-align: -1px; margin-right: 6px;"></span>Start state</div>

<div><span style="display:inline-block; width: 10px; height: 10px; border: 2px solid rgba(60,180,100,0.95); border-radius: 3px; vertical-align: -1px; margin-right: 6px;"></span>Goal state</div>

<div style="margin-top: -8px; line-height: 1.15;">

$\pi$: $\mathrm{Uniform}(\mathcal{A})$ <br>
$P$: $\mathrm{Uniform}(\mathcal{S})$ <br>
$r \equiv 1,\quad \gamma=1$

<div v-click style="position: absolute; left: 5px; top: 83px; width: 95px; height: 54px; z-index: 10;">
  <img src="./public/unknown.png" style="width: 100%; height: 100%; display: block;" />
  <div style="position: absolute; left: 87px; top: 3px; width: 100%; padding-left: 10px; text-align: left; color: black; font-size: 0.68rem; font-weight: 800; letter-spacing: 0.02em; line-height: 1.05;">
    Unknown<br>
    in RL
  </div>
</div>

</div>
</div>


<DpRlGridCompare />

---
layout: two-cols
headerEnable: true
headerTitle: Background
pageNumber: true
---


### TD Learning with Function Approximation

<div style="position: absolute; right: 82px; bottom: -5px; width: 300px; height: 200px;">
  <div v-click.hide style="position: absolute; inset: 0;">
    <div v-click style="position: absolute; inset: 0;">
      <img src="./public/mdp/step9.png" style="width: 90%; height: 100%; object-fit: contain; display: block;" />
    </div>
  </div>

  <div v-after style="position: absolute; inset: 0;">
    <img src="./public/mdp/step10.png" style="width: 90%; height: 100%; object-fit: contain; display: block;" />
  </div>
</div>

::left::

<div style="font-size: 0.92rem; line-height: 1.35; padding-right: 14px; margin-top: 28px;">

<div v-click>

#### Residual Gradient (Baird, 1995)

We approximate the value function by $V(s;\theta)$.

$$\begin{aligned}L^{*}(\theta)&=\frac{1}{2}\mathbb{E}_{S}\left[\left(V^\pi(S)-V(S;\theta)\right)^2\right]\\ &=\frac{1}{2}\mathbb{E}_{S}\left[\left(\mathbb{E}_{A,S'}\left[r+\gamma V^\pi(S')\right]-V(S;\theta)\right)^2\right]\end{aligned}$$

</div>

<div v-click>

This motivates

$$L(\theta):=\frac{1}{2}\mathbb{E}_{S}\left[\left(\mathbb{E}_{A,S'}\left[r+\gamma V(S';\theta)\right]-V(S;\theta)\right)^2\right]$$

</div>

  <div v-click>
    <div style="position: absolute; left: 165px; top: 350px; width: 150px; border-top: 1px solid red">
  </div>

  <div style="position: absolute; left: 195px; top: 350px; font-size: 0.8rem;">
    Call this term <span style="color: red;">"Target"</span>.
  </div>
</div>

<div v-click.hide>
<div v-click>
  <div style="position: absolute; left: 180px; top: 380px; font-size: 0.8rem;">

$$\begin{aligned}&\text{Recall Bellman Equation}\\ &\qquad V^\pi(s)=\mathbb{E}_{A,S'}\bigg[r(s,A)+\gamma V^\pi(S')\bigg]\end{aligned}$$

  </div>

</div>
</div>

<div v-after style="position: absolute; top: 340px; width: 70%;">

The descent direction is

<div style="position: absolute; left: 20px; top: 35px;">

$$\begin{aligned}-\nabla_\theta L(\theta)=\mathbb{E}_{S}\bigg[\bigg(\mathbb{E}_{A,{\color{red}S'}}&\left[r+\gamma V(S';\theta)\right]-V(S;\theta)\bigg)\\ &\bigg(\nabla_\theta V(S;\theta)-\gamma\mathbb{E}_{{\color{red}S'}}\left[\nabla_\theta V(S';\theta)\right]\bigg)\bigg]\end{aligned}$$

</div>

<div style="position: absolute; top: 150px;">
<strong class="danger">Double sampling issue</strong> : Unbiased sample-based gradient estimate requires <br> two independent next-state samples from the same state.
</div>

</div>


</div>




::right::

<div style="font-size: 0.92rem; line-height: 1.35; padding-left: 14px; margin-top: 28px;">


<div v-click>

#### Semi-Gradient TD (Sutton, 1988)


In semi-gradient TD, we first take the descent direction of $L^*$.

$$-\nabla_\theta L^{*}(\theta)=\mathbb{E}_{S}\left[\left(\mathbb{E}_{A,S'}\left[r+\gamma V^\pi(S')\right]-V(S;\theta)\right)\nabla_\theta V(S;\theta)\right]$$

</div>

<div v-click>

Then, replace the unknown true value by the current approximation.

$$g(\theta)=\mathbb{E}_{S}\left[\left(\mathbb{E}_{A,S'}\left[r+\gamma V(S';\theta)\right]-V(S;\theta)\right)\nabla_\theta V(S;\theta)\right]$$

</div>

<div v-click>

In general, this vector field is not conservative: $\partial_{\theta_j}(g)_i\ne\partial_{\theta_i}(g)_j$. Thus, semi-gradient TD avoids the double-sampling issue, but it is generally not the gradient flow of any objective function.

</div>

</div>


---
layout: two-cols
headerEnable: true
headerTitle: Background
pageNumber: true
---

### Why Not Use the True Gradient?

Although Residual Gradient follows the true gradient of the Bellman-residual loss, semi-gradient TD is computationally lightweight and often learns faster in practice.

::left::

#### Linear Function Approximation

<img src="./public/off-policy/linear.png" style="position: absolute; left: 107.5px; top: 220px; width: 27%; object-fit: contain; display: block;" />

<div style="position: absolute; left: 80px; top: 485px; width: 330px; font-size: 0.62rem; line-height: 1.2; color: #444; text-align: center;">
  TD is often competitive with gradient-TD alternatives in small linear benchmarks.<sup>1</sup>
</div>

::right::

#### Nonlinear Function Approximation

<img src="./public/off-policy/nonlinear.png" style="position: absolute; left: 507.5px; top: 220px; width: 30%; object-fit: contain; display: block;" />

<div style="position: absolute; left: 490px; top: 485px; width: 360px; font-size: 0.62rem; line-height: 1.2; color: #444; text-align: center;">
  With neural networks, TD can learn better policies than Residual Gradient.<sup>2</sup>
</div>

<div style="position: absolute; left: 24px; bottom: 10px; width: calc(100% - 48px); font-size: 0.5rem; line-height: 1.15; color: #666;">
  <sup>1</sup> Figure adapted from Sutton et al. (2009); selected panels shown and non-TD methods grouped visually for clarity.<br>
  <sup>2</sup> Figure adapted from Yin et al., <i>An Experimental Comparison Between Temporal Difference and Residual Gradient with Neural Network Approximation</i>.
</div>

---
layout: two-cols
headerEnable: true
headerTitle: Background
pageNumber: true
---

### Semi-Gradient TD: Optimization Perspective

<br>

::left::

<div style="font-size: 0.82rem; line-height: 1.35; padding-right: 14px;">



#### Residual Gradient

$$\begin{aligned}&\text{Fixed objective}\\ &\qquad\;\, L(\theta)=\frac{1}{2}\mathbb{E}_{S}\left[\left(\mathbb{E}_{A,S'}\left[r+\gamma V(S';\theta)\right]-V(S;\theta)\right)^2\right]\\ &\text{Direction}\\ &-\nabla_\theta L(\theta)=\mathbb{E}_{S}\bigg[\bigg(\mathbb{E}_{A,S'}\left[r+\gamma V(S';\theta)\right]-V(S;\theta)\bigg)\\ &\qquad\qquad\qquad\qquad\qquad\bigg(\nabla_\theta V(S;\theta)-\gamma\mathbb{E}_{S'}\left[\nabla_\theta V(S';\theta)\right]\bigg)\bigg]\end{aligned}$$

#### Semi-Gradient TD


$$\begin{aligned}&\text{Fixed objective}\\ &\qquad\qquad\qquad\text{No such objective in general}\\ &\text{Direction}\\ &\quad g(\theta)=\mathbb{E}_{S}\left[\left(\mathbb{E}_{A,S'}\left[r+\gamma V(S';\theta)\right]-V(S;\theta)\right)\nabla_\theta V(S;\theta)\right]\end{aligned}$$

</div>

::right::

<div style="font-size: 0.92rem; line-height: 1.0; padding-left: 10px;">


<div v-click>

#### Moving Target Perspective of Semi-Gradient TD



  <div style="position: absolute; left: 185px; top: 195px; width: 140px; border-top: 1.5px solid red; z-index: 20;"></div>

  The difference between $-\nabla_\theta L(\theta)$ and $g(\theta)$ is whether we differentiate the <span style="color: red;">target</span> term.

</div>

<div v-click>

So, if the target is frozen, the two directions coincide, and $g(\theta)$ can be viewed as the descent direction of a frozen-target objective. To express this, we separate the frozen <span style="color: red;">target parameter</span> and the <span style="color: #2563eb;">optimizing parameter</span>.

</div>

<div v-click>

$$H({\color{red}\theta},{\color{#2563eb}\omega})=\frac{1}{2}\mathbb{E}_{S,A,S'}\left[\left(r+\gamma V(S';{\color{red}\theta})-V(S;{\color{#2563eb}\omega})\right)^2\right]$$

</div>

<div v-click>

For fixed <span style="color: red;">$\theta$</span>, descent in <span style="color: #2563eb;">$\omega$</span> gives $g(\theta)=-\nabla_\omega H(\theta,\omega)\big|_{\omega=\theta}$.

</div>

<div v-click>

Updating along $g(\theta_t)$ gives
$$\theta_{t+1}=\theta_t+\alpha g(\theta_t)=\theta_t-\alpha\nabla_\omega H(\theta_t,\theta_t).$$
This is <strong class="danger">one-step optimization + moving target</strong>: optimize $H(\theta_t,\cdot)$ for one step, then move the objective to $H(\theta_{t+1},\cdot)$.

</div>


<div v-click>
  <img src="./public/greenback.png" style="position: absolute; left: 47.5px; top: 353px; width: 41%; object-fit: contain; display: block;" />

  <MovingTargetFrames />

</div>

</div>


---
layout: two-cols
headerEnable: true
headerTitle: Background
pageNumber: true
---

### On-Policy and Off-Policy TD Learning



<div style="font-size: 0.9rem; line-height: 1.35; padding-right: 16px;">

Semi-gradient TD can be written as

$$\theta_{t+1}=\theta_t-\alpha\nabla_\omega H(\theta_t,\theta_t).$$

where

$$H(\theta,\omega)=\frac{1}{2}\mathbb{E}_{\substack{S\sim d\\ A\sim\pi(\cdot\mid S)\\ S'\sim P(\cdot\mid S,A)}}\left[\left(r(S,A)+\gamma V(S';\theta)-V(S;\omega)\right)^2\right].$$

The distribution $d$ specifies where the TD error is measured.

</div>

::left::

<div style="font-size: 0.95rem; line-height: 1.35; padding-left: 0px; margin-top: 0px;">

<div v-click>

On-policy:$d$ is the stationary distribution of the Markov chain induced by the target policy $\pi$.
</div>

<div v-click style="margin-top: 22px;">
Off-policy: otherwise.

<div style="position: absolute; left: 50px; top: 455px;">
  Off-policy learning is important for sample efficiency, <br> but together with bootstrapping and function approximation, <br> it can become unstable.
</div>
</div>

</div>

::right::

<div style="font-size: 0.95rem; line-height: 1.35; padding-left: 12px; margin-top: 0px;">

<div v-click style="margin-top: 18px;">

  Even with linear function approximation, off-policy TD can diverge.<sup>1</sup>
  <img src="./public/off-policy/baird.png" style="position: absolute; right: 120px;width: 25%; object-fit: contain; display: block;" />
</div>

</div>




---
layout: two-cols
headerEnable: true
headerTitle: Background
pageNumber: true
---

## Mean-Field Regime

<br>

::left::

<div style="font-size: 0.82rem; line-height: 1.35; padding-right: 14px">

### Two-layer neural network

<div v-click.hide>
<img src="./public/2layernn/diagram.png" style="position: absolute; left: 10px; top: 130px; width: 45%; object-fit: contain; display: block;" />

<div style="font-size: 0.72rem; position: absolute; left: 170px; top: 245px;">

$$\begin{gathered}\hat{y}(x;\boldsymbol{\theta})=\frac{1}{M}\sum_{i=1}^Ma_i\sigma\left(\left\langle w_i,x\right\rangle+b_i\right)=:\frac{1}{M}\sum_{i=1}^M\phi(x;\theta_i)\\ \bigg(\theta_i:=\left(a_i,w_i,b_i\right)\in\mathbb{R}^{d}\bigg)\end{gathered}$$

</div>
</div>
<div v-after>
<img src="./public/2layernn/permutation.png" style="position: absolute; left: 10px; top: 130px; width: 45%; object-fit: contain; display: block;" />

<div style="font-size: 0.72rem; position: absolute; left: 170px; top: 245px;">

$$\begin{gathered}\hat{y}(x;\boldsymbol{\theta})=\frac{1}{M}\sum_{i=1}^Ma_i\sigma\left(\left\langle w_i,x\right\rangle+b_i\right)=:\frac{1}{M}\sum_{i=1}^M\phi(x;\theta_i)\\ \bigg(\theta_i:=\left(a_i,w_i,b_i\right)\in\mathbb{R}^{d}\bigg)\end{gathered}$$

</div>
</div>

<div v-click>

<div style="position: absolute;top: 345px;">

The output $\hat{y}$ is invariant to swapping $\theta_i$ and $\theta_j$.
This motivates the expression

$$\hat{y}(x;\boldsymbol{\theta})=\int \phi(x;\theta)\,\mu_M(\mathrm{d}\theta)\left(\mu_M=\frac{1}{M}\sum_{i=1}^M\delta_{\theta_i}\right)$$

</div>

</div>

<div v-click>

<div style="position: absolute;top: 455px;">

**Infinite width limit :** For $\mu\in\mathcal{P}(\mathbb{R}^d)$,
<div style="font-size: 0.82rem; position: absolute; left: 120px; top: 30px;">

$$\hat{y}(x;\mu)=\int \phi(x;\theta)\,\mu(\mathrm{d}\theta)$$

</div>
</div>

</div>


</div>

::right::

<div style="font-size: 0.82rem; line-height: 1.35; padding-left: 14px;">

### Supervised Learning

<div v-click>

Define the regularized population objective

$$E(\mu)=\frac{1}{2}\mathbb{E}_{(x,y)\sim\rho}\left[\left(\hat y(x;\mu)-y\right)^2\right]+\frac{\lambda}{2}\int \|\theta\|^2\,\mu(d\theta).$$

</div>

<div v-click>

Particle noisy gradient descent:

$$\theta_i^{k+1}=\theta_i^k-\eta\nabla_\theta\frac{\delta E}{\delta\mu}(\mu_k)(\theta_i^k)+\sqrt{2\tau\eta}\,\xi_i^k.$$

where $\:\xi_i^k\sim\mathcal{N}(0,I)$.
</div>

<div v-click>

Continuous-time limit:

$$\begin{aligned}d\theta_t&=-\nabla_\theta\frac{\delta E}{\delta\mu}(\mu_t)(\theta_t)\,dt+\sqrt{2\tau}\,dW_t,\\\partial_t\mu_t&=\nabla_\theta\cdot\left(\mu_t\nabla_\theta\frac{\delta E}{\delta\mu}(\mu_t)\right)+\tau\Delta_\theta\mu_t.\end{aligned}$$

</div>

<div v-click>

This is the Wasserstein gradient flow of $F(\mu):=E(\mu)+\tau\mathrm{Ent}(\mu)$.

</div>


</div>


---
layout: default
headerEnable: true
headerTitle: Background
pageNumber: true
---

### Convergence Analysis of Mean Field Langevin Dynamics
<br>

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 42px; align-items: start;">

<div v-click style="font-size: 0.78rem; line-height: 1.25; min-width: 0;">

#### Key tools

Define the local Gibbs response and the global minimizer by
$$\pi_\mu:=\operatorname*{argmin}_{\nu}\left\{\int \frac{\delta E}{\delta\mu}(\mu)(\theta)\,\nu(d\theta)+\tau\mathrm{Ent}(\nu)\right\},\, \pi_*:=\operatorname*{argmin}_{\mu}F(\mu).$$

<div class="theorem-box" style="--theorem-padding: 10px 14px; --theorem-margin-top: 15px; --theorem-font-size: 0.74rem; --theorem-line-height: 1.24; --theorem-head-margin-bottom: 6px; --theorem-body-margin-top: 6px;">
  <div class="theorem-head">
    <span class="theorem-label">Assumption.</span>
    <span class="theorem-name">Uniform Log-Sobolev Inequality</span>
  </div>

  <div class="theorem-body">

The Gibbs responses $\pi_\mu$ satisfy LSI with a uniform constant $\rho_\tau>0$:
$$\mathrm{KL}(\nu\|\pi_\mu)\leq\frac{1}{2\rho_\tau}I(\nu\|\pi_\mu),\qquad\forall\mu,\nu.$$
  </div>
</div>

<div style="margin-top: 5px; font-size: 0.64rem; line-height: 1.24; color: rgba(35, 35, 55, 0.72);">
  <b>Intuition.</b> A large gap to the Gibbs response implies a large descending force.
</div>

<div class="theorem-box" style="--theorem-padding: 10px 14px; --theorem-margin-top: 15px; --theorem-font-size: 0.74rem; --theorem-line-height: 1.24; --theorem-head-margin-bottom: 6px; --theorem-body-margin-top: 6px;">
  <div class="theorem-head">
    <span class="theorem-label">Lemma.</span>
    <span class="theorem-name">Entropy Sandwich (Informal) (Nitanda et al. (2022) and Chizat (2022))</span>
  </div>

  <div class="theorem-body">

The free-energy gap is sandwiched by relative entropy:
$$\tau\mathrm{KL}(\mu\|\pi_*)\leq F(\mu)-F(\pi_*)\leq\tau\mathrm{KL}(\mu\|\pi_\mu).$$
  </div>
</div>

<div style="margin-top: 5px; font-size: 0.64rem; line-height: 1.24; color: rgba(35, 35, 55, 0.72);">
  <b>Intuition.</b> The global optimality gap can be controlled by the distance from the current Gibbs response.
</div>

</div>

<div style="font-size: 0.78rem; line-height: 1.25; min-width: 0;">

<div v-click>

#### Analysis

<div class="theorem-box" style="--theorem-padding: 10px 14px; --theorem-margin-top: 20px; --theorem-font-size: 0.74rem; --theorem-line-height: 1.24; --theorem-head-margin-bottom: 6px; --theorem-body-margin-top: 6px;">
  <div class="theorem-head">
    <span class="theorem-label">Theorem.</span>
    <span class="theorem-name">Exponential Convergence (Informal) (Nitanda et al. (2022) and Chizat (2022))</span>
  </div>

  <div class="theorem-body">

Under suitable regularity, flat convexity, and uniform LSI, the mean-field Langevin dynamics satisfy
$$F(\mu_t)-F(\pi_*)\leq e^{-2\tau\rho_\tau t}\left(F(\mu_0)-F(\pi_*)\right).$$
  </div>
</div>

</div>

<div v-click style="margin-top: -10px;">

_Proof Sketch._

<div style="position: absolute; left: 545px; top: 310px; font-size: 0.8rem;">

Along the mean-field Langevin dynamics,
$$\begin{aligned}\frac{d}{dt}\left(F(\mu_t)-F(\pi_*)\right)&=-\tau^2 I(\mu_t\|\pi_{\mu_t})\\&\leq -2\rho_\tau\tau^2\mathrm{KL}(\mu_t\|\pi_{\mu_t})\\&\leq -2\rho_\tau\tau\left(F(\mu_t)-F(\pi_*)\right).\end{aligned}$$

Therefore, Gronwall gives
$$F(\mu_t)-F(\pi_*)\leq e^{-2\tau\rho_\tau t}\left(F(\mu_0)-F(\pi_*)\right).$$
</div>
</div>

</div>

</div>



---
layout: default
headerEnable: true
headerTitle: Background
pageNumber: true
---

## Prior Work

<br>

<div style="font-size: 0.84rem; line-height: 1.34;">

#### Linear Function Approximation

<b>On-policy.</b> Semi-gradient TD converges under standard assumptions, and finite-time/sample-efficiency analyses are also well studied.
<span style="font-size: 0.68rem; color: #666;">Tsitsiklis and Van Roy (1997); Bhandari et al. (2018)</span>


<b>Off-policy.</b> Even with linear function approximation, one can construct divergent examples; regularized variants can recover convergence in some settings.
<span style="font-size: 0.68rem; color: #666;">Baird (1995); Sutton et al. (2009); Lim and Lee (2022)</span>


#### Neural Network Approximation

<b>NTK regime.</b> Convergence guarantees and rate analyses have been developed, but the analysis is restricted to dynamics near initialization.
<span style="font-size: 0.68rem; color: #666;">Cai et al. (2019), etc</span>


<b>Mean-field regime.</b> Convergence is known when an optimal solution is sufficiently close to the initialization.
<span style="font-size: 0.68rem; color: #666;">Zhang et al. (2020)</span>


<b>Other approaches.</b> Projection-type constraints are often used to keep TD iterates bounded, but they analyze a constrained dynamics rather than the original unconstrained update.
<span style="font-size: 0.68rem; color: #666;">Xu and Gu (2020); Cayci et al. (2021); Tian et al. (2023); Gallici et al. (2025)</span>


<div style="padding: 12px 14px; border: 1.4px solid rgba(35,35,35,0.72); border-radius: 10px; background: rgba(255,255,255,0.72);">
  <b>This work.</b> We identify a stability condition for noisy mean-field semi-gradient TD with feature learning, and prove convergence rates under this condition. We also extend the framework to off-policy soft Q-learning.
</div>

</div>


---
layout: section
subject: Main Results
---

# Main Results


---
layout: two-cols
headerEnable: true
headerTitle: Our Analysis
pageNumber: true
---

### Mean-Field Semi-Gradient TD

<br>

::left::

<div style="font-size: 0.76rem; line-height: 1.27; padding-right: 16px;">


<div v-click>

#### Frozen-target objectives

Recall the frozen-target objective

$$H(\theta,\omega)=\frac{1}{2}\mathbb{E}_{\substack{S\sim d\\A\sim\pi(\cdot\mid S)\\S'\sim P(\cdot\mid S,A)}}\left[\left(r(S,A)+\gamma V(S';\theta)-V(S;\omega)\right)^2\right].$$



</div>


<div v-click>

In the mean-field limit, define

$$H(\mu,\nu)=\frac{1}{2}\mathbb{E}_{\substack{S\sim d\\A\sim\pi(\cdot\mid S)\\S'\sim P(\cdot\mid S,A)}}\left[\left(r(S,A)+\gamma V(S';\mu)-V(S;\nu)\right)^2\right],$$

$$E(\mu,\nu):=H(\mu,\nu)+\frac{\lambda}{2}\int\|\theta\|^2\nu(d\theta),$$

$$F(\mu,\nu):=E(\mu,\nu)+\tau\mathrm{Ent}(\nu).$$

</div>


<div v-click>

#### Noisy particle semi-gradient TD

$$\theta_i^{k+1}=\theta_i^k-\eta\nabla_\theta\frac{\delta E}{\delta\nu}(\mu_k,\mu_k)(\theta_i^k)+\sqrt{2\tau\eta}\,\xi_i^k.$$

where $\xi_i^k\sim\mathcal{N}(0,I)$.

</div>

</div>

::right::

<div style="font-size: 0.76rem; line-height: 1.27; padding-left: 16px;">

<div v-click>

#### Continuous-time limit

$$\begin{aligned}d\theta_t&=-\nabla_\theta\frac{\delta E}{\delta\nu}(q_t,q_t)(\theta_t)\,dt+\sqrt{2\tau}\,dW_t,\\\partial_tq_t&=\nabla_\theta\cdot\left(q_t\nabla_\theta\frac{\delta E}{\delta\nu}(q_t,q_t)\right)+\tau\Delta_\theta q_t.\end{aligned}$$

</div>


<div v-click>

For each frozen target $\mu$, define
$$\pi_{\mu,*}:=\operatorname*{argmin}_{\nu}F(\mu,\nu),\qquad \pi_{\mu,\nu}(\theta)\propto\exp\left(-\frac{1}{\tau}\frac{\delta E}{\delta\nu}(\mu,\nu)(\theta)\right).$$

The limiting equilibrium is the self-consistent minimizer
$$\pi_*=\pi_{\pi_*,*}.$$

<img src="./public/moving_target/_step07.png" style="position: absolute; right: 180px; bottom: 10px; width: 23%; object-fit: contain;" />

</div>

</div>


---
layout: default
headerEnable: true
headerTitle: Convergence Analysis
pageNumber: true
---

## Proof Strategy



<div v-click.hide style="position: absolute; left: -180px; top: 100px; width: 70%; font-size: 0.92rem;">
<div v-click>

$$\frac{d}{dt}\left(F(\mu_t,\mu_t)-F(\pi_*,\pi_*)\right)$$
</div>
</div>
<div v-after style="position: absolute; left: -60px; top: 100px; width: 70%; font-size: 0.92rem;">

$$\begin{aligned}W_2(q_t, q_*)&\leq W_2(q_t, \pi_{q_t,*})+W_2(\pi_{q_t,*}, \pi_{*})\quad(\text{triangle inequality})\\ &=:W_2(q_t, T(q_t))+W_2(T(q_t), \pi_{*})\\ &=W_2(q_t, T(q_t))+W_2(T(q_t), T(\pi_{*}))\\ &\overset{?}{\leq}W_2(q_t, T(q_t))+CW_2(q_t, \pi_*)\end{aligned}$$
</div>

<div v-click style="position: absolute; left: -160px; top: 230px; width: 70%; font-size: 0.92rem;">

$$\Longrightarrow W_2(q_t, \pi_*)\leq \frac{1}{1-C}W_2(q_t, T(q_t))$$
</div>

<div v-click style="position: absolute; left: 17px; top: 235px; width: 70%; font-size: 0.92rem;">

$$\xrightarrow{t\to\infty} 0?$$
</div>



<div v-click style="position: absolute; left: 60px; bottom: 5px; width: 40%; font-size: 0.92rem;">

<div class="theorem-box" style="--theorem-padding: 5px 14px; --theorem-margin-top: 15px; --theorem-font-size: 0.74rem; --theorem-line-height: 1.24; --theorem-head-margin-bottom: 6px; --theorem-body-margin-top: 6px;">
  <div class="theorem-head">
    <span class="theorem-label">Assumption.</span>
    <span class="theorem-name">(Mixed smoothness).</span>
  </div>

  <div class="theorem-body">

$$\left\|\nabla_\theta\left[\frac{\delta H}{\delta \mu}(\mu,\nu_1)(\theta)-\frac{\delta H}{\delta \mu}(\mu,\nu_2)(\theta)\right]\right\|_2\leq L_p W_2(\nu_1,\nu_2).$$
  </div>
</div>

</div>



<div v-click style="position: absolute; left: 460px; top: 150px; width: 70%; font-size: 0.92rem;">

<div class="theorem-box" style="--theorem-width: 65%; --theorem-padding: 10px 14px; --theorem-margin-top: 15px; --theorem-font-size: 0.74rem; --theorem-line-height: 1.24; --theorem-head-margin-bottom: 6px; --theorem-body-margin-top: 6px;">
  <div class="theorem-head">
    <span class="theorem-label">Lemma1.</span>
    <span class="theorem-name">(Stability of the Gibbs response map).</span>
  </div>

  <div class="theorem-body">

Let $T(\mu):=\pi_{\mu,*}(\theta)d\theta$ be the frozen-target Gibbs response map. Then

$$W_2(T(\mu_1),T(\mu_2))\leq \frac{L_p}{\rho\tau}W_2(\mu_1,\mu_2).$$

In particular, if $\rho\tau>L_p$, then $T$ is a contraction.
  </div>
</div>

<div style="position: absolute; left: -70px; top: 40px; font-size: 3rem; line-height: 1; color: #000000;">⟵</div>

</div>

<div v-click style="position: absolute; left: 60px; top: 310px; width: 70%; font-size: 0.92rem;">

<div class="theorem-box" style="--theorem-width: 65%; --theorem-padding: 10px 14px; --theorem-margin-top: 15px; --theorem-font-size: 0.74rem; --theorem-line-height: 1.24; --theorem-head-margin-bottom: 6px; --theorem-body-margin-top: 6px;">
  <div class="theorem-head">
    <span class="theorem-label">Theorem1.</span>
    <span class="theorem-name">(Frozen-target suboptimality gap decay).</span>
  </div>

  <div class="theorem-body">

Under the mixed-smoothness assumption and the stability condition $\rho\tau>L_p$, the one-step frozen-target suboptimality gap decays exponentially:

$$G(t):=F(q_t,q_t)-F(q_t,\pi_{q_t,*}),\qquad G(t)\leq e^{-2(\rho\tau-L_p)t}G(0).$$

  </div>
</div>

<div style="position: absolute; left: 270px; top: -45px; font-size: 2.5rem; line-height: 1; color: #000000;">↑</div>

</div>





<img src="./public/moving_target/_step07.png" style="position: absolute; right: 180px; bottom: 10px; width: 23%; object-fit: contain" />



<div v-click style="position: absolute; left: 560px; top: -10px; width: 70%; font-size: 0.92rem;">



<div class="theorem-box" style="--theorem-width: 60%; --theorem-padding: 10px 14px; --theorem-margin-top: 15px; --theorem-font-size: 0.74rem; --theorem-line-height: 1.24; --theorem-head-margin-bottom: 6px; --theorem-body-margin-top: 6px;">
  <div class="theorem-head">
    <span class="theorem-label">Corollary1.</span>
    <span class="theorem-name">(Convergence to a self-consistent equilibrium).</span>
  </div>

  <div class="theorem-body">

Under $\rho\tau>L_p$, there exists a unique self-consistent equilibrium $\pi_*$, and the noisy mean-field semi-gradient TD dynamics converges exponentially:

$$W_2(q_t,\pi_*)\leq \frac{\sqrt{2\rho\tau}}{\rho\tau-L_p}\sqrt{G(0)}e^{-(\rho\tau-L_p)t}.$$

  </div>
</div>

</div>




---
layout: default
headerEnable: true
headerTitle: Convergence Analysis
pageNumber: true
---

## Neumerical Example


<img src="./public/experiment/bellman_residual_overlay.png" style="position: absolute; right: 530px; bottom: 90px; width: 40%; object-fit: contain" />

<img src="./public/experiment/learning_curves.png" style="position: absolute; right: 100px; bottom: 90px; width: 40%; object-fit: contain" />


---
layout: section
subject: Conclusion
---

# Conclusion


---
layout: default
headerEnable: true
headerTitle: Conclusion
pageNumber: true
---

<div class="slide-title-no-toc slide-title-h3" style="position: absolute; left: 40px; top: 65px;">
Conclusion
</div>

<div class="conclusion-list-wrap">
  <ul class="conclusion-list">
    <li>We focused on semi-gradient TD with two-layer neural-network approximation.</li>
    <li>In the mean-field regime, noisy semi-gradient TD converges under a stability condition.</li>
    <li>The analysis also extends beyond policy evaluation, including entropy-regularized off-policy soft Q-learning.</li>
    <li>Future work: connect this mean-field convergence result to error bounds for the true value function.</li>
  </ul>
</div>

<style>
.conclusion-list-wrap {
  position: absolute;
  left: 45px;
  top: 115px;
  width: 820px;
}

.conclusion-list {
  font-size: 1.28rem;
  line-height: 1.45;
  padding-left: 28px;
  margin: 0;
}

.conclusion-list li {
  margin-bottom: 26px;
}

.conclusion-list li:last-child {
  margin-bottom: 0;
}

.slide-title-h3 {
  font-size: 1.13rem;
  line-height: 1.2;
  font-weight: 900;
}
</style>


---
layout: section
subject: Thank you
hideInToc: true
---
