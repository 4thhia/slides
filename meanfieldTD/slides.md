---
theme: ../common/themes/neat
routerMode: hash
layout: cover
colorSchema: light
coverTitle: "On the Convergence of Semi-Gradient TD: A Mean-Field Perspective"
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
  Still, in continuing or long-horizon tasks, such feedback is not always available, and TD learning remains a fundamental component for deep RL.
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
  <div style="position: absolute; left: 0; top: 10px; width: 100%; padding-left: 5px; text-align: left; color: white; font-size: 1.0rem; font-weight: 800; letter-spacing: 0.02em; line-height: 1.05;">
    Unknown in <br>
    RL
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

#### Gradient TD

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

#### Semi-Gradient TD


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

### Semi-Gradient TD: Optimization Perspective

<br>

::left::

<div style="font-size: 0.82rem; line-height: 1.35; padding-right: 14px;">



#### Gradient TD

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

### Semi-Gradient TD with Linear Function Approximation


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

<div style="position: absolute;top: 345px;">

The output $\hat{y}$ is invariant to swapping $\theta_i$ and $\theta_j$.
This motivates the expression

$$\begin{gathered}\hat{y}(x;\boldsymbol{\theta})=\hat{y}(x;\mu_M):=\int \phi(x;\theta)\,\mu_M(\mathrm{d}\theta)\\ \left(\mu_M=\frac{1}{M}\sum_{i=1}^M\delta_{\theta_i}\in\mathcal{P}(\mathbb{R}^d)\right)\end{gathered}$$

</div>

</div>

</div>

::right::

<div style="font-size: 0.82rem; line-height: 1.35; padding-left: 14px;">

### Infinite width limit



</div>


---
layout: two-cols
headerEnable: true
headerTitle: Background
pageNumber: true
---

### Learning Dynamics of Men-Field Network


---
layout: two-cols
headerEnable: true
headerTitle: Background
pageNumber: true
---

### Men-Field Regime for Semi-Gradient TD



---
layout: section
subject: Convergence Analysis
---

# Convergence Analysis


---
layout: default
headerEnable: true
headerTitle: Convergence Analysis
pageNumber: true
---

## Assumptions


---
layout: default
headerEnable: true
headerTitle: Convergence Analysis
pageNumber: true
---


## Main Result

**Policy evaluation**

<div class="theorem-box theorem">
  <div class="theorem-head">
    <span class="theorem-label">Theorem 1</span>
    <span class="theorem-name">(Instantaneous optimality gap decay).</span>
  </div>

  <div class="theorem-body">

Under the mixed-smoothness assumption and the stability condition $\rho\tau>L_p$, the one-step frozen-target optimality gap decays exponentially:

$$G(t):=F(q_t,q_t)-F(q_t,\pi_{q_t,*}),\qquad G(t)\leq e^{-2(\rho\tau-L_p)t}G(0).$$

  </div>
</div>

<div v-click class="theorem-comment">

The semi-gradient dynamics is not analyzed through decrease of a fixed objective.
Instead, we compare $q_t$ with the instantaneous frozen-target minimizer $\pi_{q_t,*}$.

</div>

---
layout: default
headerEnable: true
headerTitle: Convergence Analysis
pageNumber: true
---

**Policy evaluation**

<div class="theorem-box lemma">
  <div class="theorem-head">
    <span class="theorem-label">Lemma 1</span>
    <span class="theorem-name">(Stability of the Gibbs response map).</span>
  </div>

  <div class="theorem-body">

Let $T(\mu):=\pi_{\mu,*}(\theta)d\theta$ be the frozen-target Gibbs response map. Then

$$W_2(T(\mu_1),T(\mu_2))\leq \frac{L_p}{\rho\tau}W_2(\mu_1,\mu_2).$$

In particular, if $\rho\tau>L_p$, then $T$ is a contraction.

  </div>
</div>

<div v-click class="theorem-comment">

This converts the local frozen-target relaxation into stability of the moving target itself.

</div>

---
layout: default
headerEnable: true
headerTitle: Convergence Analysis
pageNumber: true
---

**Policy evaluation**

<div class="theorem-box corollary">
  <div class="theorem-head">
    <span class="theorem-label">Corollary 1</span>
    <span class="theorem-name">(Convergence to a self-consistent equilibrium).</span>
  </div>

  <div class="theorem-body">

Under $\rho\tau>L_p$, there exists a unique self-consistent equilibrium $\pi_*$, and the noisy mean-field semi-gradient TD dynamics converges exponentially:

$$W_2(q_t,\pi_*)\leq \frac{\sqrt{2\rho\tau}}{\rho\tau-L_p}\sqrt{G(0)}e^{-(\rho\tau-L_p)t}.$$

  </div>
</div>

<div v-click class="theorem-comment">

The same stability condition controls both the frozen-target optimization error and the movement of the target.

</div>

---
layout: default
headerEnable: true
headerTitle: Convergence Analysis
pageNumber: true
---

**Extension to Soft Q-learning**

<div class="theorem-box proposition">
  <div class="theorem-head">
    <span class="theorem-label">Proposition 4</span>
    <span class="theorem-name">(Soft Q-learning convergence).</span>
  </div>

  <div class="theorem-body">

For the entropy-regularized off-policy soft Q-learning functional, assume the same Gibbs-form characterization, uniform LSI, and mixed-smoothness estimate with constant $L_p^Q$. If $\rho\tau>L_p^Q$, then the Gibbs response map has a unique fixed point $\pi_*^Q$, and

$$W_2(q_t,\pi_*^Q)\leq \frac{\sqrt{2\rho\tau}}{\rho\tau-L_p^Q}\sqrt{G_Q(0)}e^{-(\rho\tau-L_p^Q)t}.$$

  </div>
</div>

<div v-click class="theorem-comment">

The same entropy-versus-sensitivity mechanism extends beyond policy evaluation.

</div>


---
layout: default
headerEnable: true
headerTitle: Convergence Analysis
pageNumber: true
---

## Neumerical Example


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
