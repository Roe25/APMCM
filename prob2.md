# 净冷却功率评估模型 - 数学公式

## 1. 核心评价指标

### 1.1 净冷却功率

$$P_{net} = P_{rad} - P_{atm} - P_{solar}$$

其中：
- $P_{rad}$ = 表面辐射功率 (W/m²)
- $P_{atm}$ = 大气吸收功率 (W/m²)
- $P_{solar}$ = 太阳吸收功率 (W/m²)

---

## 2. 各功率分量计算

### 2.1 表面辐射功率 $P_{rad}$

$$P_{rad} = \pi \int_0^{\infty} \varepsilon(\lambda) \cdot B(\lambda, T_s) \, d\lambda$$

物理意义：薄膜向外太空辐射的总功率

### 2.2 大气吸收功率 $P_{atm}$

$$P_{atm} = \pi \int_0^{\infty} \varepsilon(\lambda) \cdot \varepsilon_{atm}(\lambda) \cdot B(\lambda, T_{amb}) \, d\lambda$$

其中大气发射率：
$$\varepsilon_{atm}(\lambda) = 1 - \tau_{atm}(\lambda)$$

物理意义：薄膜从大气逆辐射中吸收的功率

### 2.3 太阳吸收功率 $P_{solar}$

$$P_{solar} = \int_{0.3}^{2.5} \varepsilon(\lambda) \cdot I_{solar}(\lambda) \, d\lambda$$

物理意义：薄膜从太阳辐射中吸收的功率

---

## 3. 基础物理公式

### 3.1 普朗克黑体辐射

$$B(\lambda, T) = \frac{2hc^2}{\lambda^5} \cdot \frac{1}{e^{hc/(\lambda k_B T)} - 1}$$

### 3.2 发射率计算（基尔霍夫定律）

$$\varepsilon(\lambda) = 1 - R(\lambda) - T(\lambda)$$

### 3.3 传输矩阵法

总传输矩阵：
$$M = D_{12} \cdot P \cdot D_{23}$$

反射率与透射率：
$$R = \left| \frac{M_{21}}{M_{11}} \right|^2, \quad T = \left| \frac{1}{M_{11}} \right|^2$$

---

## 4. 太阳光谱辐照度

$$I_{solar}(\lambda) = B(\lambda, T_{sun}) \cdot \Omega_{sun} \cdot \pi \cdot \tau_{atm,solar}$$

其中：
- $T_{sun} = 5778$ K（太阳表面温度）
- $\Omega_{sun} = \pi (R_{sun}/D_{sun})^2$（太阳立体角）
- $\tau_{atm,solar}$ = 大气对太阳光的透射率

---

## 5. 大气透射率模型

| 波长范围 | $\tau_{atm}(\lambda)$ | 说明 |
|----------|----------------------|------|
| 8-13 μm | 0.6-0.8 | 大气窗口（高透射） |
| 9.5-10 μm | ~0.6 | O₃吸收带 |
| < 8 μm | ~0.2 | H₂O强吸收 |
| > 13 μm | ~0.3 | CO₂和H₂O吸收 |

---

## 6. 最优厚度确定

### 6.1 优化目标

$$d_{opt} = \arg\max_d \left[ P_{net}(d) \right]$$

### 6.2 优化结果

对于PDMS薄膜：

$$d_{opt} \approx 85 \, \mu m$$

$$P_{net,max} \approx 131.8 \, \text{W/m}^2$$

### 6.3 功率分解（最优厚度）

| 功率分量 | 数值 | 占比 |
|----------|------|------|
| $P_{rad}$ | +267.1 W/m² | 100% (输出) |
| $P_{atm}$ | -122.6 W/m² | 45.9% (损失) |
| $P_{solar}$ | -12.7 W/m² | 4.8% (损失) |
| $P_{net}$ | =131.8 W/m² | 49.3% (净输出) |

---

## 7. 厚度对各功率分量的影响

### 7.1 $P_{rad}$ 随厚度变化

$$\frac{\partial P_{rad}}{\partial d} > 0 \quad \text{(单调递增)}$$

原因：厚度增加 → 发射率增加 → 辐射功率增加

### 7.2 $P_{atm}$ 随厚度变化

$$\frac{\partial P_{atm}}{\partial d} > 0 \quad \text{(单调递增)}$$

原因：发射率增加 → 吸收率增加（基尔霍夫定律）

### 7.3 $P_{solar}$ 随厚度变化

$$\frac{\partial P_{solar}}{\partial d} > 0 \quad \text{(单调递增)}$$

原因：厚度增加 → 太阳光吸收增加

### 7.4 $P_{net}$ 存在极值

$$\frac{\partial P_{net}}{\partial d} = \frac{\partial P_{rad}}{\partial d} - \frac{\partial P_{atm}}{\partial d} - \frac{\partial P_{solar}}{\partial d} = 0$$

在 $d = d_{opt}$ 处，$P_{net}$ 达到最大值。

---

## 8. 物理常数

| 常数 | 符号 | 数值 | 单位 |
|------|------|------|------|
| 普朗克常数 | $h$ | $6.626 \times 10^{-34}$ | J·s |
| 光速 | $c$ | $2.998 \times 10^{8}$ | m/s |
| 玻尔兹曼常数 | $k_B$ | $1.381 \times 10^{-23}$ | J/K |
| 表面温度 | $T_s$ | 300 | K |
| 环境温度 | $T_{amb}$ | 300 | K |
| 太阳温度 | $T_{sun}$ | 5778 | K |

---

## 9. 结论

### 9.1 评价指标优势

净冷却功率 $P_{net}$ 作为评价指标的优势：

1. **物理意义明确**：直接表示冷却能力（W/m²）
2. **能量守恒**：考虑完整的能量收支平衡
3. **全波段积分**：包含普朗克函数加权
4. **可优化**：存在明确的最优厚度

### 9.2 设计建议

基于净冷却功率优化结果：

- **日间冷却**：推荐厚度 50-120 μm
- **夜间冷却**：可适当增加厚度（无太阳加热）
- **最优选择**：~85 μm（平衡辐射与吸收）
