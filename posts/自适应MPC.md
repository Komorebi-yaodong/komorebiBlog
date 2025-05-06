# 具有参数不确定性的无约束离散时间线性系统自适应MPC

> zyl

针对具有参数不确定性的无约束DLTI系统，提出了一种简单自适应MPC，是通过将估计参数的自适应律和约束MPC相结合而得到的。

> 主要贡献：
>
> 1. 通过应用自适应更新律，将具有参数不确定性的无约束MPC转化为没有参数不确定性的约束MPC
> 2. 理论可以证明，采用所提出的自适应MPC，闭环系统的跟踪误差最终是有界的，估计参数也是有界的。（如果参考信号恒定，跟踪误差也可以渐进稳定）

## 1. 自适应MPC

### 1.1 系统建模与转换

待控制的原始系统是一个具有参数不确定性的离散时间线性系统：
$$
\begin{cases}
x_p(k+1) = A_p x_p(k) + B_p u_p(k) \\
y_p(k) = C_p x_p(k)
\end{cases}
$$
其中 $A_p \in \mathbb{R}^{n \times n}$ 和 $B_p \in \mathbb{R}^{n \times m}$ 是未知的常数矩阵，但假设系统 $(A_p, B_p)$ 是可控的。 $C_p \in \mathbb{R}^{l \times n}$ 是已知的。控制目标是使输出 $y_p(k)$ 跟踪有界的参考信号 $r_s(k)$。

为了便于MPC设计，系统被转换为==**增量形式**== ：
$$
\begin{cases}
x(k+1) = A x(k) + B \Delta u(k) \\
y(k) = C x(k)
\end{cases}
$$
其中 $x(k) = [\Delta x_p(k)^T, y_p(k)^T]^T$， $\Delta u(k) = u_p(k) - u_p(k-1)$。新的状态矩阵 $A$ 和输入矩阵 $B$ 包含了未知的 $A_p, B_p$，因此 $A, B$ 也是**不确定**的常数矩阵。 $C = [0_{l \times n} \quad I_{l \times l}]$ 是已知的。 $(A, B)$ 的可控性由 $(A_p, B_p)$ 的可控性保证。

**假设 1**: 定义合并后的参数矩阵 $\Theta \equiv [A, B]$。存在一个已知的保守上界 $\bar{\Theta}$，使得 $|| \Theta || \le \bar{\Theta}$。

### 1.2 自适应更新律

为了应对参数不确定性$\Theta$，设计了一个估计系统：
$$
\hat{x}(k+1) = \hat{A}(k) x(k) + \hat{B}(k) \Delta u(k) = \hat{\Theta}(k) X(k) \quad (5)
$$
其中 $\hat{A}(k), \hat{B}(k)$ (或 $\hat{\Theta}(k)$) 是对未知参数 $A, B$ (或 $\Theta$) 的**时变估计**。 $X(k) \triangleq [x(k)^T, \Delta u(k)^T]^T$ 是增广状态向量。

两者相减得到状态估计误差的动态：
$$
\tilde{x}(k+1) = x(k+1) - \hat{x}(k+1) = (\Theta - \hat{\Theta}(k)) X(k) = \tilde{\Theta}(k) X(k)
$$
为了推导更新律，定义一个关于估计误差$\tilde{x}(k+1)$的代价函数：
$$
J_e = \tilde{x}(k+1)^T \tilde{x}(k+1) = (x(k+1) - \hat{\Theta}(k) X(k))^T (x(k+1) - \hat{\Theta}(k) X(k))
$$
使用梯度法，求解代价函数对参数估计误差的梯度
$$
\nabla J_e = \frac{\partial J_e}{\partial \hat{\Theta}(k)} = -X(k) (x(k+1) - \hat{\Theta}(k) X(k))^T = -X(k) \tilde{x}(k+1)^T
$$
采用梯度下降法来最小化 $J_e$，得到参数 $\hat{\Theta}(k)$ 的**自适应更新律**:
$$
\hat{\Theta}(k+1) = \hat{\Theta}(k) - \lambda \nabla J_e^T = \hat{\Theta}(k) + \lambda \tilde{x}(k+1) X(k)^T \quad (6)
$$
==**定理 1**:== 考虑系统 (3) 及其增量形式 (4)。假设存在可行控制 $\Delta u(k)$ 使得下式成立：
$$
X(k)^T X(k) \le \frac{2-\alpha}{\lambda} \quad (7)
$$
其中 $0 < \alpha < 2$ 且 $\lambda > 0$。则使用更新律 (6)，以下结论成立：

1.  参数估计误差 $\tilde{\Theta}(k)$ 是**最终有界**的。
2.  状态估计误差 $\tilde{x}(k)$ 是**渐近稳定**的（即 $\tilde{x}(k) \to 0$ 当 $k \to \infty$）。

> 备注：①只要$X(k)$有限，总可以找到足够小的$\lambda$来满足约束
>
> ​			  ②下一步的目标是设计MPC控制器找到满足约束的$\Delta u(k)$

### 1.3 基于估计系统的MPC设计

核心思想：既然无法基于不确定的真实系统进行MPC预测，基于当前估计的系统进行预测，并设计MPC控制律，同时，将定理1中的稳定性条件（7）转化为MPC优化问题中的约束。

1. 预测方程：预测写成紧凑形式 $\hat{Y}(k) = F(k) \hat{x}(k) + \Phi(k) \Delta U \quad$

2. MPC代价函数：跟踪问题，所以有关于$Y$跟踪的惩罚项，也有控制增量$\Delta U$​的惩罚项：
   $$
   J_y = (\hat{Y}(k) - R_s(k))^T (\hat{Y}(k) - R_s(k)) + \Delta U^T \bar{R} \Delta U \quad (13)
   $$
   改写为关于 $\Delta U$ 的二次规划标准形式（忽略了与$\Delta U$无关的项），其中 $H(k) = \Phi(k)^T (F(k)\hat{x}(k) - R_s(k))$ 和 $E(k) = \Phi(k)^T \Phi(k) + \bar{R}$：
   $$
   \hat{J}_y = 2 \Delta U^T H(k) + \Delta U^T E(k) \Delta U \quad (14)
   $$

3. 约束转换：将定理1中的稳定性条件改写为对当前控制增量$\Delta u(k)$​的约束
   $$
   \Delta u(k)^T \Delta u(k) < \frac{2-\alpha}{\lambda} - x(k)^T x(k)
   $$
   假设$x(k)$可测量，这个范数约束被转化为线性不等式约束形式：$M \Delta U \le \gamma \quad (15)$

   这个约束将**自适应律的稳定性要求**和 **MPC 控制器的设计**联系起来。

4. 终端约束：等式约束 $\hat{y}(k+N_c|k) = r_s(k+N_c)$

5. MPC优化问题： 最终的 MPC 问题是在每个采样时刻 $k$ 求解以下优化问题
   $$
   \Delta U^*(k) = \arg \min_{\Delta U} (2 \Delta U^T H(k) + \Delta U^T E(k) \Delta U) \tag{18}
   $$
   约束条件：估计系统动态、预测动态、状态/输入约束（源于自适应稳定性），终端约束

6. **receding horizon 控制**: 求解得到最优控制序列 $\Delta U^*(k)$ 后，仅将第一个控制增量 $\Delta u^*(k)$ 应用于系统：
   $$
   \Delta u(k) = [I_{m \times m}, 0, ..., 0] \Delta U^*(k) \quad (19)
   $$
   在下一个采样时刻 $k+1$，测量新的状态，更新参数估计 $\hat{\Theta}(k+1)$，并重复上述优化过程。
