# Paper Summary

## Distributed Learning

### 1. Unleashing the Tiger: Inference Attacks on Split Learning (Split Learning Safety-CCS 2021)

[Paper's Link](https://arxiv.org/abs/2012.02670)



#### Introduction

It designed a new framework to permit an attacker to **recover precise reconstruction of individual clients' training instances** and **perform property inference attacks**, and these attacks can circumvent defensive techniques.

This attacker's name is **Feature-Space Hijacking Attack(FSHA)** and it aimed to the split learning training protocol.

> **Split Learning:**
>
> [Split Learning Simple Introduction](https://zhuanlan.zhihu.com/p/541880009)



#### Method

**--Threat model**

We assume that attacker does not have information on the clients participating in the distributed learning, except those required to run this model. It even do not know the model $f$ and its weights. Moreover, it do not know the task.

However, it know the dataset $X_{pub}$ that captures the same domain of clients' datasets.

**--Foundations**

The malicious server substitutes the original learning task chosen by the clients with a new objective that shapes the feature space of $f$ on purpose.

During the attack process, the **malicious server hijacks the training process and steers it towards a specific, target feature space $\tilde Z$.**

And the attack process can be divided into two phases: (1) **Setup phase**: malicious server hijacks learning process and $f$, (2) **Inference phase**: malicious server can freely recover the data sent from the clients. 

**--Setup phase**

The attacker trains three different networks $\tilde f, \tilde f^{-1}$ and $D$.

(1) $\tilde f$ dynamically defines the target feature space $\tilde Z$ for the $f$, and it is a mapping between the data space and target space where $|\tilde f(x)|=|f(x)|=k$. In a word, **$\tilde f$ learns a map between $X_{pub}$ and $\tilde Z$.**

(2) $\tilde f^{-1}$ is an approximation of the inverse function $\tilde f$ to recover the data from client to the original private data.

(3) $D$ is the discriminator that indirectly guides $f$ to learn the mapping between the private data and the feature space. Ultimately, $D$ will substitute the original server in the protocol to sent the gradients to the client. **In a word, the target of $D$ is to make $f$ and $\tilde f$ similar.**

Firstly, the server starts the training by the loss:
$$
L_{\tilde f,\tilde f^{-1}}=d(\tilde f^{-1}(\tilde f(X_{pub})), X_{pub})
\tag{1}
$$
where $d$ is a suitable distance function, e.g., MSE

Then, $D$ is trained to distinguish between the feature space induced from $\tilde f$ and one induced from the client's network $f$. $D$ takes $\tilde f(X_{pub})$ or $f(X_{priv})$ as input and is trained to assign high probability to the former and low probability to the latter:
$$
L_D=\log (1-D(\tilde f(X_{pub})) + \log (D(f(X_{priv}))
\tag{2}
$$
And $f$ receives the malicious gradients and trained by:
$$
L_f=\log(1-D(f(X_{priv})))
\tag{3}
$$
**--Inference phase**

Due to the setup phase, $f$ and $\tilde f$ are similar and we can recover the information of $X_{priv}$ by:
$$
\tilde X_{priv}=\tilde f^{-1}(f(X_{priv}))
\tag{4}
$$


#### Experiments

Seems good. Click paper's link for more information.



#### Defensive Techniques

**--Distance correlation minimization**

To minimize the information of $X_{priv}$ in smashed data:
$$
\alpha_1·DCOR(X_{priv}, f(X_{priv})) + \alpha_2·TASK(y,s(f(X_{priv})))
\tag{5}
$$
 where $DCOR$ means the distance correlation metrics, $TASK$ is the task loss of the distributed model(e.g., CE loss for classification task), $y$ is the suitable label for the task.

But this defensive technique does not work well.

**--Detecting the attack**

Seems bad.



### 2. UnSplit: Data-Oblivious Model Inversion, Model Stealing, and Label Inference Attacks Against Split Learning (Split Learning Safety-WPES 2022)

[Paper's Link](https://arxiv.org/abs/2108.09033)



#### Introduction

Propose two attacks:

(1) The first attack allows a SplitNN server **recover client's input while also obtaining a functionally similar model**. We assume that the attacker only knows the architecture of the client model.

(2) The second attack is a label inference attack that allows an honest-but-curious server to **infer the supposedly protected labels**.



#### Method

**--Threat model**

For the model inversion and model stealing attack, a full network $F$ is partitioned into two functions $f_1$ and $f_2$. We assume that the attacker knows the architecture of $f_1$ without any data similar to the training data or the ability to query the client network.

The attacker's target is to find the parameter $\tilde \theta$ and input $\tilde x$ to minimize the difference between $\tilde f_1(\tilde \theta_1,\tilde x)$ and $f_1(\theta_1,x)$ by:
$$
\tilde x^*=\arg \min_{\tilde x} MSE(\tilde f_1(\tilde \theta_1,\tilde x),f_1(\theta_1,x)) + \lambda TV(\tilde x)
\tag{2}
$$

$$
\tilde \theta_1^* = \arg \min_{\tilde \theta_1}MSE(\tilde f_1(\tilde \theta_1,\tilde x),f_1(\theta_1,x))
\tag{3}
$$





















