# 辐射冷却评估模型 - 数学公式

## 1. 薄膜光学性质

### 1.1 复折射率

$$\tilde{N} = n + ik$$

其中：
- $n$ = 折射率（实部）
- $k$ = 消光系数（虚部）

### 1.2 菲涅尔系数（正入射）

反射系数：
$$r_{12} = \frac{N_1 - N_2}{N_1 + N_2}$$

透射系数：
$$t_{12} = \frac{2N_1}{N_1 + N_2}$$

### 1.3 薄膜中的相位延迟

$$\delta = \frac{2\pi \tilde{N} d}{\lambda}$$

其中：
- $d$ = 薄膜厚度
- $\lambda$ = 波长

### 1.4 传输矩阵法

界面矩阵：
$$D_{12} = \frac{1}{t_{12}} \begin{pmatrix} 1 & r_{12} \\ r_{12} & 1 \end{pmatrix}$$

传播矩阵：
$$P = \begin{pmatrix} e^{-i\delta} & 0 \\ 0 & e^{i\delta} \end{pmatrix}$$

总传输矩阵：
$$M = D_{12} \cdot P \cdot D_{23}$$

### 1.5 反射率与透射率

$$r = \frac{M_{21}}{M_{11}}, \quad t = \frac{1}{M_{11}}$$

$$R = |r|^2, \quad T = |t|^2$$

### 1.6 发射率（基尔霍夫定律）

$$\varepsilon(\lambda) = 1 - R(\lambda) - T(\lambda)$$

---

## 2. 黑体辐射

### 2.1 普朗克光谱辐射度

$$B(\lambda, T) = \frac{2hc^2}{\lambda^5} \cdot \frac{1}{e^{hc/(\lambda k_B T)} - 1}$$

其中：
- $h = 6.626 \times 10^{-34}$ J·s（普朗克常数）
- $c = 3 \times 10^8$ m/s（光速）
- $k_B = 1.381 \times 10^{-23}$ J/K（玻尔兹曼常数）

### 2.2 斯特藩-玻尔兹曼定律

总辐射功率：
$$P = \sigma T^4$$

其中 $\sigma = 5.67 \times 10^{-8}$ W/(m²·K⁴)

---

## 3. 辐射冷却性能指标

### 3.1 大气窗口发射率 (8-13 μm)

加权平均发射率：
$$\bar{\varepsilon}_{atm} = \frac{\int_{\lambda_1}^{\lambda_2} \varepsilon(\lambda) \cdot B(\lambda, T_{amb}) \, d\lambda}{\int_{\lambda_1}^{\lambda_2} B(\lambda, T_{amb}) \, d\lambda}$$

其中 $\lambda_1 = 8$ μm，$\lambda_2 = 13$ μm，$T_{amb} = 300$ K

### 3.2 太阳光谱吸收率 (0.3-2.5 μm)

加权平均吸收率：
$$\bar{\alpha}_{solar} = \frac{\int_{\lambda_1}^{\lambda_2} \varepsilon(\lambda) \cdot I_{solar}(\lambda) \, d\lambda}{\int_{\lambda_1}^{\lambda_2} I_{solar}(\lambda) \, d\lambda}$$

其中 $\lambda_1 = 0.3$ μm，$\lambda_2 = 2.5$ μm

---

## 4. 净冷却功率计算

### 4.1 表面辐射功率

$$P_{rad} = \pi \int_0^{\infty} \varepsilon(\lambda) \cdot B(\lambda, T_s) \, d\lambda$$

### 4.2 大气辐射吸收功率

$$P_{atm} = \pi \int_0^{\infty} \varepsilon(\lambda) \cdot \varepsilon_{atm}(\lambda) \cdot B(\lambda, T_{amb}) \, d\lambda$$

其中大气发射率：
$$\varepsilon_{atm}(\lambda) = 1 - \tau_{atm}(\lambda)$$

### 4.3 太阳辐射吸收功率

$$P_{solar} = \int_{0.3}^{2.5} \varepsilon(\lambda) \cdot I_{solar}(\lambda) \, d\lambda$$

### 4.4 净辐射冷却功率

$$P_{net,rad} = P_{rad} - P_{atm}$$

### 4.5 总净冷却功率

$$P_{net,total} = P_{rad} - P_{atm} - P_{solar}$$

即：
$$P_{net,total} = P_{net,rad} - P_{solar}$$

---

## 5. 性能评估评分

### 5.1 冷却性能评分

$$Score = 50 \cdot \bar{\varepsilon}_{atm} + 50 \cdot (1 - \bar{\alpha}_{solar})$$

- 大气窗口发射率越高 → 辐射散热越强
- 太阳吸收率越低 → 太阳加热越少
- 评分范围：0-100

---

## 6. 关键物理常数

| 常数 | 符号 | 数值 | 单位 |
|------|------|------|------|
| 普朗克常数 | $h$ | $6.626 \times 10^{-34}$ | J·s |
| 光速 | $c$ | $2.998 \times 10^{8}$ | m/s |
| 玻尔兹曼常数 | $k_B$ | $1.381 \times 10^{-23}$ | J/K |
| 斯特藩-玻尔兹曼常数 | $\sigma$ | $5.67 \times 10^{-8}$ | W/(m²·K⁴) |

---

## 7. 大气窗口特性

| 波长范围 | 透射率 | 说明 |
|----------|--------|------|
| 8-13 μm | 0.6-0.8 | 主大气窗口 |
| 9.5-10 μm | ~0.6 | O₃ 吸收带 |
| < 8 μm | ~0.2 | H₂O 水汽吸收 |
| > 13 μm | ~0.3 | CO₂ 和 H₂O 吸收 |

---

## 8. 最优厚度确定

最优厚度 $d_{opt}$ 通过最大化净冷却功率确定：

$$d_{opt} = \arg\max_d \left[ P_{net,total}(d) \right]$$

对于PDMS薄膜，分析结果为：
- **最优厚度**：~142 μm
- **最大净冷却功率**：~143 W/m²
