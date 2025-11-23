# 多层膜辐射冷却优化 - 数学公式总结

## 1. 多层薄膜光学理论

### 1.1 复折射率

$$\tilde{N} = n + ik$$

其中：
- $n$ = 折射率（实部）
- $k$ = 消光系数（虚部）

---

## 2. 传输矩阵法 (Transfer Matrix Method)

### 2.1 单层传输矩阵

对于第 $i$ 层薄膜，传输矩阵为：

$$M_i = D_i \cdot P_i$$

#### 2.1.1 界面矩阵

$$D_i = \frac{1}{t_i} \begin{pmatrix} 1 & r_i \\ r_i & 1 \end{pmatrix}$$

其中菲涅尔系数（正入射）：

$$r_i = \frac{N_{i-1} - N_i}{N_{i-1} + N_i}$$

$$t_i = \frac{2N_{i-1}}{N_{i-1} + N_i}$$

#### 2.1.2 传播矩阵

$$P_i = \begin{pmatrix} e^{-i\delta_i} & 0 \\ 0 & e^{i\delta_i} \end{pmatrix}$$

相位延迟：

$$\delta_i = \frac{2\pi \tilde{N}_i d_i}{\lambda}$$

其中：
- $d_i$ = 第 $i$ 层厚度
- $\lambda$ = 波长

### 2.2 多层膜总传输矩阵

对于 $N$ 层薄膜系统：

$$M_{total} = M_1 \cdot M_2 \cdot M_3 \cdot \ldots \cdot M_N$$

即：

$$M_{total} = \prod_{i=1}^{N} (D_i \cdot P_i)$$

### 2.3 光学性质计算

#### 2.3.1 反射系数和透射系数

$$r_{total} = \frac{M_{21}}{M_{11}}$$

$$t_{total} = \frac{1}{M_{11}}$$

#### 2.3.2 反射率和透射率

$$R = |r_{total}|^2$$

$$T = |t_{total}|^2 \cdot \frac{n_{substrate}}{n_{incident}}$$

其中：
- $n_{incident}$ = 入射介质折射率（空气 = 1）
- $n_{substrate}$ = 基底折射率（空气 = 1）

#### 2.3.3 吸收率（发射率）

基于能量守恒和基尔霍夫定律：

$$\varepsilon(\lambda) = A(\lambda) = 1 - R(\lambda) - T(\lambda)$$

---

## 3. 黑体辐射理论

### 3.1 普朗克光谱辐射度

$$B(\lambda, T) = \frac{2hc^2}{\lambda^5} \cdot \frac{1}{e^{\frac{hc}{\lambda k_B T}} - 1}$$

单位：W/(m²·sr·μm)

其中：
- $h = 6.626 \times 10^{-34}$ J·s（普朗克常数）
- $c = 2.998 \times 10^8$ m/s（光速）
- $k_B = 1.381 \times 10^{-23}$ J/K（玻尔兹曼常数）
- $T$ = 温度（K）

### 3.2 斯特藩-玻尔兹曼定律

$$P = \sigma T^4$$

其中 $\sigma = 5.67 \times 10^{-8}$ W/(m²·K⁴)

---

## 4. 辐射冷却性能评估

### 4.1 辐射功率（向外太空）

$$P_{rad} = \pi \int_0^{\infty} \varepsilon(\lambda) \cdot B(\lambda, T_s) \, d\lambda$$

物理意义：表面向半球空间辐射的总功率

### 4.2 大气辐射吸收功率

$$P_{atm} = \pi \int_0^{\infty} \varepsilon(\lambda) \cdot \varepsilon_{atm}(\lambda) \cdot B(\lambda, T_{amb}) \, d\lambda$$

其中大气发射率：

$$\varepsilon_{atm}(\lambda) = 1 - \tau_{atm}(\lambda)$$

大气透射率模型：

$$\tau_{atm}(\lambda) = \begin{cases}
0.8 & 8 \leq \lambda \leq 13 \, \mu m \text{ (大气窗口)} \\
0.6 & 9.5 \leq \lambda \leq 10 \, \mu m \text{ (臭氧吸收)} \\
0.2 & \lambda < 8 \, \mu m \text{ (水汽吸收)} \\
0.3 & \lambda > 13 \, \mu m \text{ (CO}_2\text{吸收)}
\end{cases}$$

### 4.3 太阳辐射吸收功率

$$P_{solar} = \int_{0.3}^{2.5} \varepsilon(\lambda) \cdot I_{solar}(\lambda) \, d\lambda$$

简化太阳光谱模型：

$$I_{solar}(\lambda) = \begin{cases}
\frac{P_{sun}}{2.2} \approx 455 \, \text{W/(m}^2\text{·}\mu\text{m)} & 0.3 \leq \lambda \leq 2.5 \, \mu m \\
0 & \text{其他}
\end{cases}$$

其中 $P_{sun} = 1000$ W/m²（标准太阳辐照度）

### 4.4 净冷却功率

$$\boxed{P_{net} = P_{rad} - P_{atm} - P_{solar}}$$

**优化目标**：最大化 $P_{net}$

---

## 5. 遗传算法优化框架

### 5.1 问题定义

**设计变量**：

$$\mathbf{x} = \{(M_1, d_1), (M_2, d_2), \ldots, (M_N, d_N)\}$$

其中：
- $M_i \in \mathcal{M}$ = 材料选择，$\mathcal{M} = \{PDMS, Ag, Al, SiO_2, TiO_2\}$
- $d_i \in [d_{min}, d_{max}]$ = 厚度（μm）
- $N \in [N_{min}, N_{max}]$ = 层数

**目标函数**：

$$\max_{\mathbf{x}} \quad f(\mathbf{x}) = P_{net}(\mathbf{x})$$

**约束条件**：

1. PDMS约束：
$$\exists \, i \in \{1, 2, \ldots, N\} : M_i = PDMS$$

2. 厚度约束：
$$0.01 \leq d_i \leq 10 \quad \forall i$$

3. 层数约束：
$$2 \leq N \leq 8$$

### 5.2 个体编码

每个个体表示一个可行解：

$$Individual = \begin{bmatrix}
M_1 & d_1 \\
M_2 & d_2 \\
\vdots & \vdots \\
M_N & d_N
\end{bmatrix}$$

### 5.3 适应度函数

$$Fitness(Individual) = P_{net}(Individual)$$

计算流程：
1. 根据结构计算 $\varepsilon(\lambda)$（传输矩阵法）
2. 计算 $P_{rad}$, $P_{atm}$, $P_{solar}$
3. 计算 $P_{net} = P_{rad} - P_{atm} - P_{solar}$

### 5.4 遗传操作

#### 5.4.1 选择（锦标赛选择）

从种群中随机选择 $k$ 个个体（$k=3$），选择适应度最高者：

$$Individual_{selected} = \arg\max_{i \in Tournament} Fitness(Individual_i)$$

#### 5.4.2 交叉（单点交叉）

对于两个父代 $P_1$ 和 $P_2$：

$$P_1 = [(M_1, d_1), \ldots, (M_i, d_i)] | [(M_{i+1}, d_{i+1}), \ldots, (M_N, d_N)]$$

$$P_2 = [(M'_1, d'_1), \ldots, (M'_j, d'_j)] | [(M'_{j+1}, d'_{j+1}), \ldots, (M'_M, d'_M)]$$

交换切点后的片段：

$$C_1 = [(M_1, d_1), \ldots, (M_i, d_i)] | [(M'_{j+1}, d'_{j+1}), \ldots, (M'_M, d'_M)]$$

$$C_2 = [(M'_1, d'_1), \ldots, (M'_j, d'_j)] | [(M_{i+1}, d_{i+1}), \ldots, (M_N, d_N)]$$

交叉概率：$P_c = 0.7$

#### 5.4.3 变异

**材料变异**（概率 $P_m = 0.2$）：

$$M_i \rightarrow M_j, \quad M_j \sim Uniform(\mathcal{M})$$

**厚度变异**（概率 $P_m = 0.2$）：

$$d_i \rightarrow d_i \times \alpha, \quad \alpha \sim Uniform[0.5, 1.5]$$

修正到可行域：

$$d_i = \max(d_{min}, \min(d_{max}, d_i))$$

**层数变异**（概率 $P_m \times 0.5 = 0.1$）：

- 添加层：
$$N \rightarrow N+1, \quad \text{插入} \, (M_{new}, d_{new})$$

- 删除层（保证PDMS约束）：
$$N \rightarrow N-1, \quad \text{删除第} \, i \, \text{层}$$

### 5.5 精英保留策略

每代保留适应度最高的 $n_e$ 个个体：

$$Elite = \{I_1, I_2, \ldots, I_{n_e}\}$$

其中：

$$n_e = \lfloor PopSize \times 0.1 \rfloor$$

$$Fitness(I_1) \geq Fitness(I_2) \geq \ldots \geq Fitness(I_{n_e})$$

---

## 6. 算法流程

### 6.1 遗传算法伪代码

```
输入：PopSize, MaxGen, P_c, P_m
输出：BestIndividual

1. 初始化种群 Population(0)
2. 评估适应度 Fitness(Population(0))
3. For gen = 1 to MaxGen:
   a. 选择：Selected = Tournament_Selection(Population(gen-1))
   b. 交叉：Offspring = Crossover(Selected, P_c)
   c. 变异：Offspring = Mutation(Offspring, P_m)
   d. 评估：Fitness(Offspring)
   e. 精英保留：Population(gen) = Elite + Offspring
   f. 更新最优解：BestIndividual = argmax Fitness
4. Return BestIndividual
```

### 6.2 迭代公式

第 $g$ 代种群：

$$Pop(g) = Elite(g-1) \cup \{Mutation(Crossover(Selection(Pop(g-1))))\}$$

收敛判据：

$$|Fitness_{best}(g) - Fitness_{best}(g-1)| < \epsilon$$

或达到最大代数：$g = MaxGen$

---

## 7. 性能指标

### 7.1 大气窗口发射率（8-13 μm）

$$\bar{\varepsilon}_{atm} = \frac{1}{\lambda_2 - \lambda_1} \int_{\lambda_1}^{\lambda_2} \varepsilon(\lambda) \, d\lambda$$

其中 $\lambda_1 = 8$ μm，$\lambda_2 = 13$ μm

加权平均（普朗克加权）：

$$\bar{\varepsilon}_{atm,w} = \frac{\int_{\lambda_1}^{\lambda_2} \varepsilon(\lambda) \cdot B(\lambda, T) \, d\lambda}{\int_{\lambda_1}^{\lambda_2} B(\lambda, T) \, d\lambda}$$

### 7.2 太阳吸收率（0.3-2.5 μm）

$$\bar{\alpha}_{solar} = \frac{1}{\lambda_2 - \lambda_1} \int_{\lambda_1}^{\lambda_2} \varepsilon(\lambda) \, d\lambda$$

其中 $\lambda_1 = 0.3$ μm，$\lambda_2 = 2.5$ μm

---

## 8. 优化结果

### 8.1 最优结构

$$\mathbf{x}^* = \begin{bmatrix}
PDMS & 1.772 \\
PDMS & 76.401 \\
PDMS & 76.401 \\
Al & 2.538 \\
PDMS & 60.015
\end{bmatrix} \, \mu m$$

总厚度：

$$d_{total} = \sum_{i=1}^{5} d_i = 217.127 \, \mu m$$

### 8.2 性能指标

| 指标 | 公式 | 单层PDMS | 优化多层膜 |
|------|------|----------|------------|
| $P_{rad}$ | $\pi \int \varepsilon B dλ$ | 267.1 W/m² | 314.9 W/m² |
| $P_{atm}$ | $\pi \int \varepsilon \varepsilon_{atm} B dλ$ | 122.6 W/m² | 155.8 W/m² |
| $P_{solar}$ | $\int \varepsilon I_{solar} dλ$ | 12.7 W/m² | 0.3 W/m² |
| **$P_{net}$** | $P_{rad} - P_{atm} - P_{solar}$ | **131.8 W/m²** | **158.8 W/m²** |
| $\bar{\varepsilon}_{atm}$ | 平均发射率(8-13μm) | 0.936 | 0.977 |
| $\bar{\alpha}_{solar}$ | 平均吸收率(0.3-2.5μm) | 0.283 | 0.000 |

### 8.3 性能提升

$$\Delta P_{net} = \frac{P_{net}^{opt} - P_{net}^{base}}{P_{net}^{base}} \times 100\% = \frac{158.8 - 131.8}{131.8} \times 100\% = 20.5\%$$

$$\Delta \bar{\alpha}_{solar} = \frac{0.000 - 0.283}{0.283} \times 100\% = -100\%$$

$$\Delta \bar{\varepsilon}_{atm} = \frac{0.977 - 0.936}{0.936} \times 100\% = 4.4\%$$

---

## 9. 物理机制分析

### 9.1 多层膜干涉效应

总反射率：

$$R_{total}(\lambda) = \left| \sum_{i=1}^{N} r_i e^{i\phi_i} \right|^2$$

其中 $\phi_i$ 是第 $i$ 界面的相位。

### 9.2 选择性光谱响应

**太阳光谱区域（0.3-2.5 μm）**：

- Al层高反射：$R_{Al} > 0.95$
- PDMS高透明：$T_{PDMS} > 0.9$
- 结果：$R_{total} > 0.95$，$\varepsilon \approx 0$

**大气窗口（8-13 μm）**：

- PDMS多层干涉增强：$\varepsilon \approx 0.98$
- Al层不影响（因PDMS高吸收）
- 结果：高发射率

### 9.3 厚度优化原理

$$\frac{dP_{net}}{dd_i} = \frac{\partial P_{rad}}{\partial d_i} - \frac{\partial P_{atm}}{\partial d_i} - \frac{\partial P_{solar}}{\partial d_i}$$

最优厚度满足：

$$\frac{dP_{net}}{dd_i}\bigg|_{d_i = d_i^*} = 0 \quad \forall i$$

---

## 10. 算法性能

### 10.1 收敛性

第 $g$ 代最优适应度：

$$F_{best}(g) = \max_{i \in Pop(g)} Fitness(Individual_i)$$

收敛速度：

- 第0代：$F_{best}(0) = 152.4$ W/m²
- 第60代：$F_{best}(60) = 157.5$ W/m²
- 第99代：$F_{best}(99) = 158.8$ W/m²

改进率：

$$\eta(g) = \frac{F_{best}(g) - F_{best}(0)}{F_{best}(0)} \times 100\%$$

最终改进率：$\eta(99) = 4.2\%$

### 10.2 计算复杂度

**单个体评估**：$O(N_{layers} \times N_{wavelength})$

**单代计算**：$O(PopSize \times N_{layers} \times N_{wavelength})$

**总计算量**：$O(MaxGen \times PopSize \times N_{layers} \times N_{wavelength})$

本研究：$O(100 \times 50 \times 8 \times 1000) \approx 4 \times 10^7$ 次基本操作

---

## 参考常数表

| 常数 | 符号 | 数值 | 单位 |
|------|------|------|------|
| 普朗克常数 | $h$ | $6.626 \times 10^{-34}$ | J·s |
| 光速 | $c$ | $2.998 \times 10^{8}$ | m/s |
| 玻尔兹曼常数 | $k_B$ | $1.381 \times 10^{-23}$ | J/K |
| 斯特藩-玻尔兹曼常数 | $\sigma$ | $5.67 \times 10^{-8}$ | W/(m²·K⁴) |
| 表面温度 | $T_s$ | 300 | K |
| 环境温度 | $T_{amb}$ | 300 | K |
| 太阳辐照度 | $P_{sun}$ | 1000 | W/m² |
