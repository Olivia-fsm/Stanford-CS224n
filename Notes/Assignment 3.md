# Assignment 3

---

## Part(a) Adam Optimizer

#### Intro.

- Original Paper: [https://arxiv.org/pdf/1412.6980.pdf](https://arxiv.org/pdf/1412.6980.pdf)

#### Algorithm

![image-20210116092307000](.\image-20210116092307000.png)



#### Problem 1.(1) 

![image-20210116093613382](.\image-20210116093613382.png)

- ##### Solution:

We can consider $m$ as the accumulated gradient, which indicates the previous moving direction. By the factor $$\beta_1$$, the updated movement combines the previous moving direction and the current computed gradient, which weakens the effects of the current gradient. 



#### Problem 1.(2)

![image-20210116094122616](.\image-20210116094122616.png)

- ##### Solution:

1. ***Mathematical Intuition:*** the divided term $$\sqrt{v}$$ is greater than 1 if $$v = \beta_2 \cdot v + (1-\beta_2) \nabla_{\theta} J_{minibatch}(\theta) \odot \nabla_{\theta} J_{minibatch}(\theta) > 1,  $$which is prone to the **Explode Gradient Problem**; while $$\sqrt{v}$$ is less than 1 if $$v = \beta_2 \cdot v + (1-\beta_2) \nabla_{\theta} J_{minibatch}(\theta) \odot \nabla_{\theta} J_{minibatch}(\theta) < 1,$$which is prone to the **Vanishing Gradient Problem**.
2. ***Adaptively Updated Learning Rate:*** When the gradient $$\nabla_{\theta} J_{minibatch}(\theta) < 1$$, the updated learning rate $$\alpha / \sqrt{v}$$ tends to be greater than 1, which makes the update step bigger. Otherwise, the updated learning rate $$\alpha / \sqrt{v}$$ tends to be greater than 1, which makes the update step shrink.
3. ***Effectively reduce the potential Vanishing Gradient, and Explode Gradient Problem.***



## Part(b). Dropout

#### Intro. 

- Original Paper: [https://www.cs.toronto.edu/˜hinton/absps/JMLRdropout.pdf](https://www.cs.toronto.edu/˜hinton/absps/JMLRdropout.pdf)

#### Demonstration

![image-20210116111342703](.\image-20210116111342703.png)

![image-20210116111424175](.\image-20210116111424175.png)

- **Question:** 所以Dropout并没有使神经元完全失活？
  - if $$y==0$$, $$\sigma(W\cdot y + b)=\sigma(b)\approx 0.5?$$
- **Answer:** 理解问题。Dropout使得上一层的某个神经元对下一层所有神经元的输出贡献=0 （即失活）， 下一层神经元当然不会失活啦

---

![image-20210116102617356](.\image-20210116102617356.png)

![image-20210116103638853](.\image-20210116103638853.png)

#### Problem 2.

![image-20210116104104998](.\image-20210116104104998.png)

##### (i.)

$$\gamma = 1/p_{drop}$$

##### (ii.)

- Through Training Process, the Dropout serves as training a collections of $$2^n$$ thinned networks with extensive weight sharing, while in Test Process, it is not feasible to explicitly average the predictions from exponentially many thinned models.



## Part(c) Dependency Parsing

#### Background

![image-20210116113830508](.\image-20210116113830508.png)



#### Problem 3.1

![image-20210116113914108](.\image-20210116113914108.png)

​           

| Stack                          | Buffer                | New Dependency                   | Transition  |
| ------------------------------ | --------------------- | -------------------------------- | ----------- |
| [ROOT, parsed, this]           | [sentence, correctly] |                                  | `SHIFT`     |
| [ROOT, parsed, this, sentence] | [correctly]           |                                  | `SHIFT`     |
| [ROOT, parsed, sentence]       | [correctly]           | sentence $$\rightarrow$$ this    | `RIGHT-ARC` |
| [ROOT, parsed]                 | [correctly]           | parsed $$\rightarrow$$ sentence  | `LEFT-ARC`  |
| [ROOT, parsed, correctly]      | [ ]                   |                                  | `SHIFT`     |
| [ROOT, parsed]                 | [ ]                   | parsed $$\rightarrow$$ correctly | `LEFT-ARC`  |
|                                |                       |                                  |             |

 ![image-20210116124130791](.\image-20210116124130791.png)

- Solution: $$(2n-1)$$





















### 2. Neural Transition-Based Dependency Parsing

#### (a)

  We can't remove parsed at the first step otherwise it wll disobey the condition.

$$ \begin{array}{|l|l|l|l} Stack & Buffer & New dependency & Transition \ \hline [Root, parsed] & [this, sentence, correctly] & parsed \rightarrow I & LEFT-ARC \ [Root, parsed, this] & [sentence, correctly] & & SHIFT \ [Root, parsed, this, sentence] & [correctly] & & SHIFT \ [Root, parsed, sentence] & [correctly] & this \rightarrow sentence & LEFT-ARC \ [Root, parsed] & [correctly] & parsed \rightarrow sentence & RIGHT-ARC \ [Root, parsed, correctly] & [] & & SHIFT \ [Root, parsed] & [] & parsed \rightarrow correctly & RIGHT-ARE \ [Root] & [] & Root \rightarrow parsed & RIGHT-ARE \end{array}$$

#### (b)

  $n-1$ steps.   There is and only is a dependency between two words, and every word only relate to a dependency. So there is $n-1$ steps.(I don't consider ROOT, otherwise it's n steps)

#### (c)

  See in [a3](https://github.com/ZacBi/CS224n-2019-solutions/blob/master/assignments/written part/a3) folder

#### (d)

$\mathrm{i.}$ 