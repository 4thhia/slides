---
theme: ../common/themes/neat
addons:
  - '@/../common/addons/slidev-addon-click-memory'
  - '@/../common/addons/slidev-addon-rabbit-minimal'
routerMode: hash
layout: cover
colorSchema: light

rabbit:
  totalDuration: 2400
duration: 0

coverTitle: |
  On the Convergence of Semi-Gradient TD:
  A Mean-Field Perspective

coverAuthor: Haruki Settai
coverCollaborator: "Tadashi Kozuno"
coverSupervisor: "Shinji Ito"

lineNumbers: true
---


---
layout: section
subject: Background
---



---
layout: default
headerEnable: true
headerTitle: Background
pageNumber: true
---

# Reinforcement Learning


### Markov Decision Process


<div style="position: absolute; left: 15px; top: 125px; --text-display-math: 1.1rem;">

$$\begin{alignedat}{3}&\text{State Space}\quad&\mathcal{S}&\subseteq\mathbb{R}^n,\;\text{bounded}\\ &\text{Action Space}\quad&\mathcal{A}&\subseteq\mathbb{R}^m,\;\text{bounded}\\ &\text{Reward}\quad&r&:\mathcal{S}\times\mathcal{A}\to[0,1]\\ \qquad&\text{Transition Model}\quad&P&:\mathcal{S}\times\mathcal{A}\to\mathcal{P}(\mathcal{S})\\ &\text{Policy}\quad&\pi&:\mathcal{S}\to\mathcal{P}(\mathcal{A})\\ \end{alignedat}$$

</div>

<img src="./public/mdp/mdp.svg" style="position: absolute; left: 480px; top: 120px; width: 47%; object-fit: contain; display: block;" />

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>


<div v-click>

### Value function & Bellman Equation


<div style="--text-display-math: 1.0rem;">

$$\begin{aligned}\text{Value function}\quad V^\pi(s):=\mathbb{E}_{\substack{A_t\sim\pi(\cdot\mid S_t)\\ S_{t+1}\sim P(\cdot\mid S_t,A_t)}}\left[\sum_{t=0}^\infty \gamma^tr(S_t,A_t)\bigg|S_0=s\right],\quad\gamma\in(0,1)\end{aligned}$$

</div>

</div>

<div v-click style="position: absolute; left: 165px;--text-display-math: 1.0rem;">

$$\begin{aligned}\text{Bellman Equation}\quad V^\pi(s)=\mathbb{E}_{\substack{A_t\sim\pi(\cdot\mid s)\\ S'\sim P(\cdot\mid s,A_t)}}\bigg[r(s,A)+\gamma V^\pi(S')\bigg]\end{aligned}$$

</div>



---
layout: default
headerEnable: true
headerTitle: Background
pageNumber: true
---

# How to Learn Value function?

### Monte Carlo method

<div style="position: absolute; left: 125px;--text-display-math: 0.95rem;">

$$L(\theta):=\frac{1}{2}\mathbb{E}_{S}\left[\left(\mathbb{E}_{\substack{A_t\sim\pi(\cdot\mid S_t)\\ S_{t+1}\sim P(\cdot\mid S_t,A_t)}}\left[\sum_{t=0}^\infty \gamma^tr(S_t,A_t)\bigg|S_0=s\right]-V(S;\theta)\right)^2\right]$$

$$\theta_{t+1}\leftarrow\theta_t-\eta \nabla_\theta L(\theta_t)$$

</div>

<img src="./public/mdp/sample_monte_carlo.svg" style="position: absolute; left: 675px; top: 170px; width: 30%; object-fit: contain; display: block;" />


<br>
<br>
<br>
<br>
<br>
<br>

### Temporal Difference Method

<div style="position: absolute; top: 290px;left: 65px;">

#### Full-Gradient TD

</div>

<br>
<br>

<div style="position: absolute; left: 125px;--text-display-math: 1.0rem;">

$$L(\theta):=\frac{1}{2}\mathbb{E}_{S}\left[\left(\mathbb{E}_{A,S'}\left[r+\gamma V(S';\theta)\right]-V(S;\theta)\right)^2\right]$$

$$\theta_{t+1}\leftarrow\theta_t-\eta \nabla_\theta L(\theta_t)$$


</div>


<img src="./public/mdp/sample_full_gradient.svg" style="position: absolute; left: 675px; top: 300px; width: 22%; object-fit: contain; display: block;" />


<br>
<br>
<br>
<br>
<br>

<div style="position: absolute; left: 65px;">

#### Semi-Gradient TD

</div>

<br>

<div style="position: absolute; left: 125px;--text-display-math: 1.0rem;">


$$H(\theta,\omega):=\frac{1}{2}\mathbb{E}_{S}\left[\left(\mathbb{E}_{A,S'}\left[r+\gamma V(S';\theta)\right]-V(S;\omega)\right)^2\right]$$


$$\begin{aligned}\omega_{t+1}&\leftarrow\omega_t-\eta \nabla_\omega H(\theta_t, \omega_t),\quad\theta_{t+1}\leftarrow\omega_{t+1}\end{aligned}$$

</div>

<img src="./public/mdp/sample_semi_gradient.svg" style="position: absolute; left: 675px; top: 450px; width: 22%; object-fit: contain; display: block;" />


---
layout: default
headerEnable: true
headerTitle: Background
pageNumber: true
---

# More on Semi Gradient TD

#### Semi-Gradient TD


<div style="position: absolute; left: 125px;--text-display-math: 1.0rem;">


$$H(\theta,\omega):=\frac{1}{2}\mathbb{E}_{S}\left[\left(\mathbb{E}_{A,S'}\left[r+\gamma V(S';\theta)\right]-V(S;\omega)\right)^2\right]$$


$$\begin{aligned}\omega_{t+1}&\leftarrow\omega_t-\eta \nabla_\omega H(\theta_t, \omega_t),\quad\theta_{t+1}\leftarrow\omega_{t+1}\end{aligned}$$

</div>


<br>
<br>
<br>
<br>
<br>
<br>

<MovingTargetFrames
  style="left: 680px; top: 70px;"
/>

#### Semi-Gradient TD is Genellary Not a Gradient of Any Function


<div style="position: absolute; top: 270px; left: 245px; --text-display-math: 1.0rem;">


$$\theta_{t+1}\leftarrow\theta_t-\eta g(\theta_t)$$

</div>

<div style="position: absolute; top: 300px; left: 55px; font-size: 1.2rem;">

where

</div>

<div style="position: absolute; top: 320px; left: 105px; --text-display-math: 1.0rem;">

$$g(\theta)=-\mathbb{E}_{S}\left[\left(\mathbb{E}_{A,S'}\left[r+\gamma V(S';\theta)\right]-V(S;\theta)\right)\nabla_\theta V(S;\theta)\right]$$

</div>

<br>
<br>
<br>
<br>
<br>
<br>


<div style="position: absolute; top: 390px; left: 55px; font-size: 1.2rem; --text-inline-math: 1.2rem; --text-display-math: 1.2rem;">

If $g(\theta)=\nabla_\theta F(\theta)$, then $\partial_j g_i(\theta)=\partial_i g_j(\theta)$. However,

</div>

<div style="position: absolute; top: 430px; left: 105px; --text-display-math: 1.0rem;">

$$\partial_j g_i(\theta)-\partial_i g_j(\theta)=\gamma\mathbb{E}\left[\partial_i V(S';\theta)\partial_j V(S;\theta)-\partial_j V(S';\theta)\partial_i V(S;\theta)\right]\neq 0$$


</div>


<img src="./public/intro/rotation.png" style="position: absolute; left: 735px; top: 300px; width: 18%; object-fit: contain; display: block;" />

<div style="
  position: absolute;
  left: 735px;
  top: 495px;
  width: 18%;
  font-size: 0.55rem;
  text-align: center;
">
  Source: Brandfonbrener & Bruna (2020), Fig. 1
</div>


---
layout: default
headerEnable: true
headerTitle: Background
pageNumber: true
---

# Contributions

<div class="comparison-table">

|  | Neural<br>Network | Feature<br>learning | Last-iterate<br>convergence | Off-policy<br>extension |
|---|:---:|:---:|:---:|:---:|
| Cai et al. (2019) | <span class="check">✓</span> | — | — | <span class="check">✓</span> |
| Zhang et al. (2020) | <span class="check">✓</span> | <span class="check">✓</span> | — | <span class="check">✓</span> |
| Agazzi and Lu (2022) | <span class="check">✓</span> | — | <span class="check">✓</span> | — |
| Asadi et al. (2023) | — | — | <span class="check">✓</span> | <span class="check">✓</span> |
| **Ours** | <span class="check">✓</span> | <span class="check">✓</span> | <span class="check">✓</span> | <span class="check">✓</span> |

</div>

<style>
.comparison-table {
  margin-top: 40px;
}

.comparison-table table {
  width: 100%;
  font-size: 0.9rem;
}

.comparison-table th {
  text-align: center;
  font-weight: 700;
}

.comparison-table td:first-child,
.comparison-table th:first-child {
  text-align: left;
}

.check {
  color: #16a34a;
  font-weight: 700;
}
</style>

<br>

<div style="position: absolute; font-size: 1.1rem;">

- In off-policy setting, The policy being evaluated is different from the policy used to collect the data.

- Our convergence guarantee relies on **regularization**, which introduces bias
  relative to the original unregularized Bellman solution.
  We quantify this bias explicitly.

</div>


---
layout: default
headerEnable: true
headerTitle: Background
pageNumber: true
---

# Mean-Field Regime

<div style="font-size: 0.82rem; line-height: 1.35; padding-right: 14px">

### Two-layer neural network

<div v-click.hide>
<img src="./public/2layernn/diagram.png" style="position: absolute; left: 30px; top: 117px; width: 56%; object-fit: contain; display: block;" />

<div style="font-size: 1.72rem; position: absolute; left: 40px; top: 385px; --text-display-math: 1.0rem;">

$$\begin{gathered}\hat{y}(x;\boldsymbol{\theta})=\frac{1}{M}\sum_{i=1}^Ma_i\sigma\left(\left\langle w_i,x\right\rangle+b_i\right)=:\frac{1}{M}\sum_{i=1}^M\phi(x;\theta_i)\\ \bigg(\theta_i:=\left(a_i,w_i,b_i\right)\in\mathbb{R}^{d}\bigg)\end{gathered}$$

</div>
</div>
<div v-after>
<img src="./public/2layernn/permutation.png" style="position: absolute; left: 30px; top: 117px; width: 56%; object-fit: contain; display: block;" />

<div style="font-size: 0.72rem; position: absolute; left: 40px; top: 385px; --text-display-math: 1.0rem;">

$$\begin{gathered}\hat{y}(x;\boldsymbol{\theta})=\frac{1}{M}\sum_{i=1}^Ma_i\sigma\left(\left\langle w_i,x\right\rangle+b_i\right)=:\frac{1}{M}\sum_{i=1}^M\phi(x;\theta_i)\\ \bigg(\theta_i:=\left(a_i,w_i,b_i\right)\in\mathbb{R}^{d}\bigg)\end{gathered}$$

</div>

</div>

<div v-click>

<div style="position: absolute; left: 500px; top: 270px; font-size: 1.2rem; --text-inline-math: 1.3rem; --text-display-math: 1.0rem;">

The output $\hat{y}$ is invariant to swapping $\theta_i$ and $\theta_j$.
This motivates representing the network through the empirical measure

$$\hat{y}(x;\boldsymbol{\theta})=\int \phi(x;\theta)\,\mu_M(\mathrm{d}\theta),
\qquad\mu_M=\frac{1}{M}\sum_{i=1}^M\delta_{\theta_i}.$$

</div>

</div>

<div v-click>

<div style="position: absolute; left: 500px; top: 425px; font-size: 1.2rem; --text-inline-math: 1.3rem; --text-display-math: 1.0rem;">

As $M\to\infty$, the empirical measure $\mu_M$ is replaced by a probability
measure $\mu\in\mathcal{P}(\mathbb{R}^d)$, giving

$$
\hat{y}(x;\mu)
=
\int \phi(x;\theta)\,\mu(\mathrm{d}\theta).
$$

</div>

</div>


</div>



---
layout: default
headerEnable: true
headerTitle: Background
pageNumber: true
---

# Example: Supervised Learning

<div style="position: absolute; left: 50px; top: 100px; font-size: 1.2rem;">

#### Regularized objective function

</div>

<div style="position: absolute; left: 80px; top: 125px; font-size: 1.2rem;--text-display-math: 1.0rem;--text-inline-math: 1.0rem;">

$$E(\mu)=\frac{1}{2}\mathbb{E}_{(x,y)\sim\rho}\left[\left(\hat y(x;\mu)-y\right)^2\right]+\frac{\lambda}{2}\int \|\theta\|^2\,\mu(d\theta),\quad F(\mu)=E(\mu)+\tau\mathrm{Ent}(\mu).$$


</div>

<div style="position: absolute; left: 50px; top: 185px; font-size: 1.2rem; --text-inline-math: 1.2rem;">

where $\tau\mathrm{Ent}(\mu)=\int\log{\left(\frac{d\mu}{d\theta}(\theta)\right)}\mu(d\theta)$

</div>

<div style="position: absolute; left: 50px; top: 230px; font-size: 1.2rem;">

#### Parameter update rule

</div>

<div style="position: absolute; left: 80px; top: 250px; --text-display-math: 1.0rem;">

$$\theta_i^{k+1}=\theta_i^k-\eta\nabla_\theta\frac{\delta E}{\delta\mu}(\mu_k)(\theta_i^k)+\sqrt{2\tau\eta}\,\xi_i^k,\qquad \xi_i^k\sim\mathcal{N}(0,I),\qquad \mu_k=\frac{1}{M}\sum_{i=1}^M\delta_{\theta_i^k}.$$

</div>


<div style="position: absolute; left: 50px; top: 330px; font-size: 1.2rem;">

#### Continuous-time limit

</div>

<div style="position: absolute; left: 80px; top: 355px; --text-display-math: 1.0rem;">

$$\begin{aligned}\mathrm{d}\theta_t&=-\nabla_\theta\frac{\delta E}{\delta\mu}(\mu_t)(\theta_t)\,\mathrm{d}t+\sqrt{2\tau}\,\mathrm{d}W_t,\\[4pt]\partial_t\mu_t&=\nabla_\theta\cdot\left(\mu_t\nabla_\theta\frac{\delta F}{\delta\mu}(\mu_t)\right),\qquad \mu_t=\operatorname{Law}(\theta_t).\end{aligned}$$

</div>


<div style="position: absolute; left: 550px; top: 330px; font-size: 1.2rem; --text-inline-math: 1.2rem;-text-display-math: 1.2rem;">

Continuity equation: $\partial_t\mu_t+\nabla_\theta\cdot(\mu_t v_t)=0$

</div>

<div style="position: absolute; left: 550px; top: 365px; font-size: 1.2rem; --text-inline-math: 1.3rem;--text-display-math: 1.2rem;">

$\Longrightarrow v_t(\theta)=-\nabla_\theta\frac{\delta F}{\delta\mu}(\mu_t)(\theta)$

</div>


<div style="position: absolute; left: 550px; top: 395px; font-size: 1.2rem; --text-inline-math: 1.3rem;--text-display-math: 1.2rem;">

$\Longrightarrow \frac{d}{dt}F(\mu_t)=-\int\left\|\nabla_\theta\frac{\delta F}{\delta\mu}(\mu_t)(\theta)\right\|^2\mu_t(d\theta).$

</div>


<div style="position: absolute; left: 650px; top: 445px; font-size: 1.2rem; --text-inline-math: 1.3rem;--text-display-math: 0.9rem;">

$$\boxed{\begin{aligned}\frac{d}{dt}\bigl(f(x_t)-f^\star\bigr)=&-\|\nabla f(x)\|^2\\ \overset{\text{PL condition}}{\leq}& -2\alpha\bigl(f(x_t)-f^\star\bigr).\end{aligned}}$$

</div>


<div style="position: absolute; left: 50px; top: 495px; font-size: 1.2rem; --text-inline-math: 1.2rem;">

Thus, the mean-field dynamics are the Wasserstein gradient flow of $F$.

</div>


---
layout: default
headerEnable: true
headerTitle: Background
pageNumber: true
---

# Example: Supervised Learning


<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 42px; align-items: start;">

<div v-click style="font-size: 0.78rem; line-height: 1.25; min-width: 0;">

#### Key tools

Define the minimizer of the linearized objective and the global minimizer by
$$\pi_\mu:=\operatorname*{argmin}_{\nu}\left\{\int \frac{\delta E}{\delta\mu}(\mu)(\theta)\,\nu(d\theta)+\tau\mathrm{Ent}(\nu)\right\},\, \pi_*:=\operatorname*{argmin}_{\mu}F(\mu).$$


<div class="theorem-box" style="--theorem-padding: 10px 14px; --theorem-margin-top: 15px; --theorem-font-size: 0.74rem; --theorem-line-height: 1.24; --theorem-head-margin-bottom: 6px; --theorem-body-margin-top: 6px;">
  <div class="theorem-head">
    <span class="theorem-label">Assumption.</span>
    <span class="theorem-name">Uniform Log-Sobolev Inequality</span>
  </div>

  <div class="theorem-body">

There exists $\rho_{\lambda,\tau}>0$ s.t.

$$\mathrm{KL}(\nu\|\pi_\mu)\le\frac{1}{2\rho_{\lambda,\tau}}I(\nu\|\pi_\mu),\qquad \forall\,\mu,\nu.$$

where $I(\nu\|\pi_\mu)=\int\|\nabla_\theta\log(d\nu/d\pi_\mu)\|^2\,d\nu$.
  </div>
</div>

<div style="margin-top: 5px; font-size: 0.64rem; line-height: 0.1; color: rgba(35, 35, 55, 0.72);">
  <b>Intuition.</b> A large gap to the linearized optimum implies a large descending force.
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

<div style="margin-top: 5px; font-size: 0.64rem; line-height: 0.1; color: rgba(35, 35, 55, 0.72);">
  <b>Intuition.</b> The Bregman divergence of entropy is KL divergence.
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
$$F(\mu_t)-F(\pi_*)\leq e^{-2\tau\rho_{\lambda,\tau} t}\left(F(\mu_0)-F(\pi_*)\right).$$
  </div>
</div>

</div>

<div v-click style="margin-top: -10px;">

_Proof Sketch._

<div style="position: absolute; left: 545px; top: 310px; font-size: 0.8rem;">

Along the mean-field Langevin dynamics,
$$\begin{aligned}\frac{d}{dt}\left(F(\mu_t)-F(\pi_*)\right)&=-\tau^2 I(\mu_t\|\pi_{\mu_t})\\&\overset{\text{LSI}}{\leq} -2\rho_{\lambda,\tau}\tau^2\mathrm{KL}(\mu_t\|\pi_{\mu_t})\\&\overset{\substack{\text{Entropy}\\ \text{Sandwich}}}{\leq} -2\rho_{\lambda,\tau}\tau\left(F(\mu_t)-F(\pi_*)\right).\end{aligned}$$

Therefore, Gronwall gives
$$F(\mu_t)-F(\pi_*)\leq e^{-2\tau\rho_{\lambda,\tau} t}\left(F(\mu_0)-F(\pi_*)\right).$$
</div>
</div>

</div>

</div>



---
layout: section
subject: Main Results
---


---
layout: two-cols
headerEnable: true
headerTitle: Main Results
pageNumber: true
---

# Mean-Field Semi-Gradient TD



<div style="position: absolute; left: 50px; top: 100px; font-size: 1.2rem;">

#### Regularized objective function

</div>

<div style="position: absolute; left: 80px; top: 125px; font-size: 1.2rem;--text-display-math: 1.0rem;--text-inline-math: 1.0rem;">

$$E(\mu,\nu)=\frac{1}{2}\mathbb{E}_{S}\left[\left(\mathbb{E}_{A,S'}\left[r+\gamma V(S';\mu)\right]-V(S;\nu)\right)^2\right]+\frac{\lambda}{2}\int \|\theta\|^2\,\nu(d\theta),\quad F(\mu)=E(\mu)+\tau\mathrm{Ent}(\nu),$$


</div>

<div style="position: absolute; left: 50px; top: 185px; font-size: 1.2rem; --text-inline-math: 1.2rem;">

where $\mathrm{Ent}(\nu)=\int\log{\left(\frac{d\nu}{d\theta}(\theta)\right)}\nu(d\theta)$.

</div>

<div style="position: absolute; left: 50px; top: 230px; font-size: 1.2rem;">

#### Parameter update rule

</div>

<div style="position: absolute; left: 80px; top: 250px; --text-display-math: 1.0rem;">

$$\theta_i^{k+1}=\theta_i^k-\eta\nabla_\theta\frac{\delta E}{\delta\nu}(\mu_k,\mu_k)(\theta_i^k)+\sqrt{2\tau\eta}\,\xi_i^k,\quad\text{where}\quad\xi_i^k\sim\mathcal{N}(0,I).$$

</div>


<div style="position: absolute; left: 420px; top: 315px; --text-inline-math: 1.3rem;">

Here, $\frac{\delta F}{\delta\mu}$ and $\frac{\delta F}{\delta \nu}$ denote variations with respect to the first and second arguments.

</div>


<div style="position: absolute; left: 50px; top: 325px; font-size: 1.2rem;">

#### Continuous-time limit

</div>

<div style="position: absolute; left: 80px; top: 345px; --text-display-math: 1.0rem;">

$$\begin{aligned}d\theta_t&=-\nabla_\theta\frac{\delta E}{\delta\nu}(q_t,q_t)(\theta_t)\,dt+\sqrt{2\tau}\,dW_t,\\\partial_tq_t&=\nabla_\theta\cdot\left(q_t\nabla_\theta\frac{\delta F}{\delta\nu}(q_t,q_t)\right).\end{aligned}$$

</div>



<div style="position: absolute; left: 50px; top: 455px; font-size: 1.2rem; --text-inline-math: 1.2rem; --text-display-math: 1.0rem;">

Unlike supervised learning, this is **not** the Wasserstein gradient flow of $q\mapsto F(q,q)$, since

$$\frac{\delta}{\delta q}F(q,q)=\frac{\delta F}{\delta\mu}(q,q)+\frac{\delta F}{\delta\nu}(q,q).$$

</div>


---
layout: two-cols
headerEnable: true
headerTitle: Main Results
pageNumber: true
---

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 42px; align-items: start;">

<div style="min-width: 0;">

## Main Result 1

<div class="theorem-box" style="--theorem-padding: 10px 14px; --theorem-margin-top: 15px; --theorem-font-size: 0.74rem; --theorem-line-height: 1.24; --theorem-head-margin-bottom: 6px; --theorem-body-margin-top: 6px;">
  <div class="theorem-head">
    <span class="theorem-label">Assumption.</span>
    <span class="theorem-name">Mixed Smoothness</span>
  </div>

  <div class="theorem-body">

There exists $L_p>0$ s.t.

$$\left\|\nabla_\theta\left[\frac{\delta H}{\delta\mu}(\mu,\nu_1)(\theta)-\frac{\delta H}{\delta\mu}(\mu,\nu_2)(\theta)\right]\right\|\le L_pW_2(\nu_1,\nu_2),\qquad \forall\,\mu,\nu_1,\nu_2,\theta.$$

  </div>
</div>

<div style="margin-top: 1px; font-size: 0.64rem; --text-inline-math: 0.64rem; line-height: 0.1; color: rgba(35, 35, 55, 0.72);">
  <b>Intuition.</b> $L_p$ measures the sensitivity to the moving bootstrap target.
</div>

<div style="margin-top: 14px;">

Define

$$T(\mu):=\operatorname*{argmin}_{\nu}F(\mu,\nu),\qquad G(t):=F(\nu_t,\nu_t)-F(\nu_t,T(\nu_t)).$$

</div>

<div class="theorem-box" style="--theorem-padding: 10px 14px; --theorem-margin-top: 15px; --theorem-font-size: 0.74rem; --theorem-line-height: 1.24; --theorem-head-margin-bottom: 6px; --theorem-body-margin-top: 6px;">
  <div class="theorem-head">
    <span class="theorem-label">Theorem1.</span>
    <span class="theorem-name">Exponential Convergence of Semi-Gradient TD (Informal)</span>
  </div>

  <div class="theorem-body">

Under suitable regularity, uniform LSI, and mixed smoothness, if

$$\tau\rho_{\lambda,\tau}>L_p,$$

then $T$ has a unique fixed point $\nu^\star=T(\nu^\star)$ and

$$W_2(\nu_t,\nu^\star)\le\frac{\sqrt{2\tau\rho_{\lambda,\tau}}}{\tau\rho_{\lambda,\tau}-L_p}\sqrt{G(0)}\,e^{-(\tau\rho_{\lambda,\tau}-L_p)t}.$$

  </div>
</div>

</div>

<div style="min-width: 0;">

<div style="min-width: 0;">

#### Proof Structure

<div class="theorem-box" style="--theorem-padding: 10px 14px; --theorem-margin-top: 15px; --theorem-font-size: 0.72rem; --theorem-line-height: 1.22; --theorem-head-margin-bottom: 6px; --theorem-body-margin-top: 6px;">
  <div class="theorem-head">
    <span class="theorem-label">Lemma 1.</span>
    <span class="theorem-name">Decay of the Frozen-Target Gap</span>
  </div>

  <div class="theorem-body">

If $\tau\rho_{\lambda,\tau}>L_p$, then

$$\frac{d}{dt}G(t)\le -2\left(\tau\rho_{\lambda,\tau}-L_p\right)G(t),$$

and hence

$$G(t)\le e^{-2(\tau\rho_{\lambda,\tau}-L_p)t}G(0).$$

  </div>
</div>


<div class="theorem-box" style="--theorem-padding: 10px 14px; --theorem-margin-top: 15px; --theorem-font-size: 0.72rem; --theorem-line-height: 1.22; --theorem-head-margin-bottom: 6px; --theorem-body-margin-top: 6px;">
  <div class="theorem-head">
    <span class="theorem-label">Lemma 2.</span>
    <span class="theorem-name">Stability of the Moving Optimum</span>
  </div>

  <div class="theorem-body">

For every $\mu_1,\mu_2$,

$$W_2\!\left(T(\mu_1),T(\mu_2)\right)\le\frac{L_p}{\tau\rho_{\lambda,\tau}}W_2(\mu_1,\mu_2).$$

Hence, if $\tau\rho_{\lambda,\tau}>L_p$, then $T$ is a contraction and admits a unique fixed point $\nu^\star=T(\nu^\star)$.

  </div>
</div>


<div style="margin-top: 18px; font-size: 0.74rem; line-height: 1.25; text-align: center;">

$$\begin{aligned}&\underset{\text{Lemma }1}{\boxed{\nu_t\text{ approaches }T(\nu_t)}}\quad+\underset{\text{Lemma }2}{\quad\boxed{T(\nu_t)\text{ is closer to }\nu^\star\text{ than }\nu_t}}\\ &\qquad\qquad\qquad\quad\Longrightarrow\text{Theorem }1\end{aligned}$$

</div>

</div>


</div>

</div>

---
layout: default
headerEnable: true
headerTitle: Main Results
pageNumber: true
---

### Lemma 1

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 48px; align-items: start;">

<div style="min-width: 0; font-size: 0.80rem; line-height: 1.28;">

<div class="theorem-box" style="--theorem-padding: 9px 14px; --theorem-margin-top: 10px; --theorem-font-size: 0.78rem; --theorem-line-height: 1.24; --theorem-head-margin-bottom: 6px; --theorem-body-margin-top: 6px;">
  <div class="theorem-head">
    <span class="theorem-label">Lemma 1.</span>
    <span class="theorem-name">Decay of the Frozen-Target Gap</span>
  </div>

  <div class="theorem-body">

If $\tau\rho_{\lambda,\tau}>L_p$, then

$$\frac{d}{dt}G(t)\le -2(\tau\rho_{\lambda,\tau}-L_p)G(t),\qquad G(t)\le e^{-2(\tau\rho_{\lambda,\tau}-L_p)t}G(0).$$

  </div>
</div>

<div style="margin-top: 16px;">

**Key idea.** Instead of differentiating $F(\nu_t,\nu_t)$ directly, track the frozen-target gap

$$G(t)=F(\nu_t,\nu_t)-F(\nu_t,T(\nu_t)).$$

</div>

<div style="margin-top: 18px;">

#### Exact Decomposition

Differentiating $G(t)$ using the envelope theorem gives

$$\frac{d}{dt}G(t)=-\underbrace{\|v_t^{\mathrm{opt}}\|_{L^2(\nu_t)}^2}_{\text{optimization}}-\underbrace{\langle v_t^{\mathrm{mov}},v_t^{\mathrm{opt}}\rangle_{L^2(\nu_t)}}_{\text{moving-target effect}}.$$

Here

$$v_t^{\mathrm{opt}}:=\nabla_\theta\frac{\delta F}{\delta\nu}(\nu_t,\nu_t),\qquad v_t^{\mathrm{mov}}:=\nabla_\theta\left[\frac{\delta E}{\delta\mu}(\nu_t,\nu_t)-\frac{\delta E}{\delta\mu}(\nu_t,T(\nu_t))\right].$$

</div>

</div>

<div style="min-width: 0; font-size: 0.80rem; line-height: 1.28;">

#### Compare the Two Effects

**LSI + entropy sandwich**

$$\|v_t^{\mathrm{opt}}\|_{L^2(\nu_t)}^2\ge 2\tau\rho_{\lambda,\tau}G(t).$$

<div style="margin-top: 22px;">

**Mixed smoothness + Talagrand + entropy sandwich**

$$\|v_t^{\mathrm{mov}}\|_{L^2(\nu_t)}\le\frac{L_p}{\tau\rho_{\lambda,\tau}}\|v_t^{\mathrm{opt}}\|_{L^2(\nu_t)}.$$

</div>

<div style="margin-top: 24px;">

Therefore,

$$\frac{d}{dt}G(t)\le-\left(1-\frac{L_p}{\tau\rho_{\lambda,\tau}}\right)\|v_t^{\mathrm{opt}}\|_{L^2(\nu_t)}^2.$$

and hence

$$\frac{d}{dt}G(t)\le-2(\tau\rho_{\lambda,\tau}-L_p)G(t).$$

</div>

<div style="margin-top: 26px; text-align: center; font-size: 0.86rem;">

<div style="margin-top: 26px; text-align: left; font-size: 0.82rem; line-height: 1.25;">

If $\tau\rho_{\lambda,\tau}>L_p$, the optimization effect is stronger than the moving-target effect, so the frozen-target gap decreases exponentially.

</div>

</div>

</div>

</div>

---
layout: default
headerEnable: true
headerTitle: Main Results
pageNumber: true
---

### Lemma 2

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 48px; align-items: start;">

<div style="min-width: 0; font-size: 0.80rem; line-height: 1.28;">

<div class="theorem-box" style="--theorem-padding: 9px 14px; --theorem-margin-top: 10px; --theorem-font-size: 0.78rem; --theorem-line-height: 1.24; --theorem-head-margin-bottom: 6px; --theorem-body-margin-top: 6px;">
  <div class="theorem-head">
    <span class="theorem-label">Lemma 2.</span>
    <span class="theorem-name">Stability of the Moving Optimum</span>
  </div>

  <div class="theorem-body">

$$W_2(T(\mu_1),T(\mu_2))\le \frac{L_p}{\tau\rho_{\lambda,\tau}}W_2(\mu_1,\mu_2).$$

  </div>
</div>

<div style="margin-top: 16px;">

**Key idea.** Lemma 1 shows that $\nu_t$ approaches its instantaneous optimum $T(\nu_t)$.
For convergence, we also need the optimum itself to move stably.

</div>

<div style="margin-top: 18px;">

#### Compare the Two Optima

We want to control

$$W_2(T(\mu_1),T(\mu_2))$$

by the change $W_2(\mu_1,\mu_2)$.

Since $T(\mu_1)$ and $T(\mu_2)$ minimize two different frozen-target objectives, introduce the mixed increment

$$\Delta H:=\bigl[H(\mu_2,T(\mu_1))-H(\mu_1,T(\mu_1))\bigr]-\bigl[H(\mu_2,T(\mu_2))-H(\mu_1,T(\mu_2))\bigr].$$

<div style="margin-top: 14px;">

This compares the effect of changing the target $\mu_1\to\mu_2$ at the two different optima.

</div>

</div>

</div>

<div style="min-width: 0; font-size: 0.80rem; line-height: 1.28;">

#### Bound the Same Quantity from Two Sides

**Lower bound: entropy sandwich + Talagrand**

The optimality of $T(\mu_1)$ and $T(\mu_2)$ gives

$$\tau\rho_{\lambda,\tau}W_2^2(T(\mu_1),T(\mu_2))\le \Delta H.$$

<div style="margin-top: 24px;">

**Upper bound: mixed smoothness**

The change of the objective with respect to its target is controlled by

$$\Delta H\le L_p\,W_2(\mu_1,\mu_2)\,W_2(T(\mu_1),T(\mu_2)).$$

</div>

<div style="margin-top: 26px;">

Combining the two bounds yields

$$W_2(T(\mu_1),T(\mu_2))\le\frac{L_p}{\tau\rho_{\lambda,\tau}}W_2(\mu_1,\mu_2).$$

</div>

<div style="margin-top: 26px; text-align: left; font-size: 0.82rem; line-height: 1.25;">

If $\tau\rho_{\lambda,\tau}>L_p$, the moving-optimum map $T$ is a contraction.

</div>

</div>

</div>

---
layout: default
headerEnable: true
headerTitle: Main Results
pageNumber: true
---

## Main Result 2

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 48px; align-items: start;">

<div style="min-width: 0; font-size: 0.80rem; line-height: 1.26;">

<div class="theorem-box" style="--theorem-padding: 8px 13px; --theorem-margin-top: 8px; --theorem-font-size: 0.76rem; --theorem-line-height: 1.20; --theorem-head-margin-bottom: 5px; --theorem-body-margin-top: 4px;">
  <div class="theorem-head">
    <span class="theorem-label">Theorem 2.</span>
    <span class="theorem-name">Approximation and Regularization Error</span>
  </div>

  <div class="theorem-body">

Let $\nu^\star$ be the self-consistent equilibrium. Then

$$\begin{aligned}\|V_{\nu^\star}-V^\pi\|_{L^2(d^\pi)}\le\inf_\nu\Bigg\{&\frac{1+\gamma}{1-\gamma}\|V_\nu-V^\pi\|_{L^2(d^\pi)}\\&+\sqrt{\frac{\tau}{1-\gamma}\mathrm{KL}(\nu\|g_{\lambda,\tau})}\Bigg\},\end{aligned}$$

where $g_{\lambda,\tau}=\mathcal N(0,\frac{\tau}{\lambda}I)$.
  </div>
</div>

<div style="margin-top: 18px;">

#### How Can We Control the Bias?

The equilibrium $\nu^\star$ does not solve the Bellman equation directly.
But it **does** minimize the frozen-target objective:

$$\nu^\star\in\operatorname*{argmin}_\nu F(\nu^\star,\nu).$$

So compare $\nu^\star$ with an arbitrary parameter law $\nu$, and convert this optimality condition into an inequality for $V_{\nu^\star}-V^\pi$.

</div>

</div>

<div style="min-width: 0; font-size: 0.80rem; line-height: 1.26;">

#### From Optimality to Bellman Error

The regularization terms can be written as

$$R(\nu)+\tau\mathrm{Ent}(\nu)=\tau\mathrm{KL}(\nu\|g_{\lambda,\tau})+\mathrm{const}.$$

First-order optimality of $\nu^\star$ therefore gives

$$\left\langle V_{\nu^\star}-T^\pi V_{\nu^\star},\,V_\nu-V_{\nu^\star}\right\rangle_{L^2(d^\pi)}+\tau\Big[\mathrm{KL}(\nu\|g_{\lambda,\tau})-\mathrm{KL}(\nu^\star\|g_{\lambda,\tau})\Big]\ge0.$$

Since $V^\pi=T^\pi V^\pi$,

$$V_{\nu^\star}-T^\pi V_{\nu^\star}=(I-\gamma P^\pi)(V_{\nu^\star}-V^\pi).$$

<div style="margin-top: 18px;">

Because $d^\pi$ is stationary,

$$\|P^\pi f\|_{L^2(d^\pi)}\le\|f\|_{L^2(d^\pi)},$$

hence

$$\left\langle(I-\gamma P^\pi)e,e\right\rangle\ge(1-\gamma)\|e\|^2,\qquad \|(I-\gamma P^\pi)e\|\le(1+\gamma)\|e\|,$$

with $e:=V_{\nu^\star}-V^\pi$.

Substituting these bounds into the optimality inequality yields Theorem 2.

</div>

</div>

</div>


---
layout: default
headerEnable: true
headerTitle: Further Background
pageNumber: true
---

# Soft Q-Learning

### Value Function and Q Function

<div style="position: absolute; left: 45px; top: 125px; width: 91%; --text-display-math: 1.0rem;">

$$\begin{aligned}\text{Value function}\qquad V^\pi(s)&:=\mathbb{E}^\pi\left[\sum_{t=0}^\infty \gamma^t r(S_t,A_t)\bigg|S_0=s\right],\\[8pt]\text{Q function}\qquad Q^\pi(s,a)&:=\mathbb{E}^\pi\left[\sum_{t=0}^\infty \gamma^t r(S_t,A_t)\bigg|S_0=s,\ A_0=a\right].\end{aligned}$$

</div>



<div v-click style="position: absolute; left: 45px; top: 300px; width: 91%;">

### Soft Bellman Equation

<div style="--text-display-math: 1.0rem;">

$$Q(s,a)=\mathbb{E}_{S'\sim P(\cdot\mid s,a)}\left[r(s,a)+\gamma V_Q^\beta(S')\right].$$

</div>

<div style="position: absolute; top: 100px; left: 10px; font-size: 1.2rem; --text-inline-math: 1.2rem;">

where $V_Q^\beta(s):=\beta\log\int_{\mathcal A}\exp\left(\frac{Q(s,a)}{\beta}\right)da$.


</div>

<div style="position: absolute; top: 150px; left: 10px; font-size: 1.2rem; --text-inline-math: 1.2rem;">

With off-policy data, the Markov operator need not be non-expansive in the data-weighted $L^2$ norm, and convergence of semi-gradient learning with nonlinear function approximation is not guaranteed in general.

</div>

</div>





---
layout: default
headerEnable: true
headerTitle: Main Results
pageNumber: true
---

# Soft Q-Learning

<div style="font-size: 1.0rem; line-height: 1.24; margin-bottom: 14px; --text-display-math: 1.0rem;">



Define

$$T_Q(\mu):=\operatorname*{argmin}_{\nu}F_Q(\mu,\nu),\qquad G_Q(t):=F_Q(\nu_t,\nu_t)-F_Q(\nu_t,T_Q(\nu_t)).$$

</div>

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 48px; align-items: start;">

<div style="min-width: 0; font-size: 0.78rem; line-height: 1.24;">

<div class="theorem-box" style="--theorem-padding: 10px 14px; --theorem-margin-top: 8px; --theorem-font-size: 0.76rem; --theorem-line-height: 1.22; --theorem-head-margin-bottom: 6px; --theorem-body-margin-top: 6px;">
  <div class="theorem-head">
    <span class="theorem-label">Theorem 3.</span>
    <span class="theorem-name">Exponential Convergence of Soft Q-Learning</span>
  </div>

  <div class="theorem-body">

Assume mixed smoothness with constant $L_p^Q$ and uniform LSI with constant $\rho$.

If

$$\rho\tau>L_p^Q,$$

then $T_Q$ has a unique fixed point $\nu_\star^Q$ and

$$W_2(\nu_t,\nu_\star^Q)\le\frac{\sqrt{2\rho\tau}}{\rho\tau-L_p^Q}\sqrt{G_Q(0)}\,e^{-(\rho\tau-L_p^Q)t}.$$

  </div>
</div>

<div style="margin-top: 18px;">

The proof has the same structure as policy evaluation: decay of $G_Q(t)$ and contraction of $T_Q$.

</div>

</div>

<div style="min-width: 0; font-size: 0.78rem; line-height: 1.24;">

<div class="theorem-box" style="--theorem-padding: 10px 14px; --theorem-margin-top: 8px; --theorem-font-size: 0.76rem; --theorem-line-height: 1.22; --theorem-head-margin-bottom: 6px; --theorem-body-margin-top: 6px;">
  <div class="theorem-head">
    <span class="theorem-label">Theorem 4.</span>
    <span class="theorem-name">Approximation and Regularization Error</span>
  </div>

  <div class="theorem-body">

Assume finite $\mathcal S,\mathcal A$ and full support

$$d_{\min}:=\min_{s,a}d_b(s,a)>0.$$

Let $Q_\beta^\star$ be the fixed point of the soft Bellman operator. Then

$$\begin{aligned}\|Q_{\nu_\star^Q}-Q_\beta^\star\|_\infty\le\frac{1}{(1-\gamma)\sqrt{d_{\min}}}\inf_\nu\Bigg\{&\|Q_\nu-T_\beta Q_{\nu_\star^Q}\|_{L^2(d_b)}\\&+\sqrt{\tau\,\mathrm{KL}(\nu\|g_{\lambda,\tau})}\Bigg\}.\end{aligned}$$

  </div>
</div>

<div style="margin-top: 18px;">

The $L^2(d_b)$ Bellman residual is converted to a sup-norm error using full support and the sup-norm contraction of the soft Bellman operator.

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


<img src="./public/experiment/minatar_td.svg" style="position: absolute; right: 130px; bottom: 90px; width: 80%; object-fit: contain" />


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

# Conclusion


<div style="position: absolute; top: 100px; left: 55px; font-size: 1.2rem; --text-inline-math: 1.2rem;">


- Semi-gradient TD is not a gradient flow because the bootstrap target moves with the parameters.
- In the mean-field regime, convergence is guaranteed when the optimization strength $\tau\rho$ dominates the moving-target sensitivity $L_p$.
- The proof combines tracking of the frozen-target optimum with stability of the moving optimum.
- The limiting equilibrium may be biased, but its error relative to the true value function can be quantified.
- The same stability mechanism extends to off-policy soft Q-learning.

</div>

---
layout: section
subject: Thank you
hideInToc: true
---
