# PDMS薄膜发射率数学模型 - 完整公式推导

## 系统结构

```
    n₀ = 1.0 (空气)
    ════════════════════  界面0: 空气/PDMS
    n₁ = ñ = n + ik      PDMS薄膜 (厚度 d)
    ════════════════════  界面1: PDMS/基底
    n₂ = n_sub           透明基底 (厚度 d_sub)
    ════════════════════  界面2: 基底/空气
    n₃ = 1.0 (空气)
```

---

## 一、基础光学常数

### 1.1 复折射率

PDMS薄膜的复折射率：

```math
ñ₁(λ) = n(λ) + ik(λ)
```

其中：
- `n(λ)` - 折射率（实部），由数据文件插值得到
- `k(λ)` - 消光系数（虚部），由数据文件插值得到
- `λ` - 真空中的波长（单位：μm）

### 1.2 插值方法

```math
n(λ) = interp_{cubic}(λ, n_{data})
k(λ) = interp_{cubic}(λ, k_{data})
```

采用三次样条插值（cubic spline）

---

## 二、菲涅尔公式

### 2.1 垂直入射（θ = 0）

#### 反射系数（振幅）

```math
r_{ij} = \frac{n_i - n_j}{n_i + n_j}
```

#### 透射系数（振幅）

```math
t_{ij} = \frac{2n_i}{n_i + n_j}
```

其中 `i, j` 表示界面两侧的介质。

### 2.2 斜入射（θ ≠ 0）

#### 斯涅尔定律

```math
n_i \sin\theta_i = n_j \sin\theta_j
```

```math
\sin\theta_t = \frac{n_i}{n_j} \sin\theta_i
```

```math
\cos\theta_t = \sqrt{1 - \sin^2\theta_t} = \sqrt{1 - \left(\frac{n_i}{n_j}\right)^2 \sin^2\theta_i}
```

#### s偏振（垂直偏振）

```math
r_s = \frac{n_i \cos\theta_i - n_j \cos\theta_t}{n_i \cos\theta_i + n_j \cos\theta_t}
```

```math
t_s = \frac{2n_i \cos\theta_i}{n_i \cos\theta_i + n_j \cos\theta_t}
```

#### p偏振（平行偏振）

```math
r_p = \frac{n_j \cos\theta_i - n_i \cos\theta_t}{n_j \cos\theta_i + n_i \cos\theta_t}
```

```math
t_p = \frac{2n_i \cos\theta_i}{n_j \cos\theta_i + n_i \cos\theta_t}
```

#### 非偏振光（平均）

```math
r = \frac{r_s + r_p}{2}
```

```math
t = \frac{t_s + t_p}{2}
```

---

## 三、薄膜干涉理论

### 3.1 相位变化

光在PDMS薄膜中传播的相位变化：

```math
\delta_1 = \frac{2\pi \tilde{n}_1 d}{\lambda} = \frac{2\pi (n + ik) d}{\lambda}
```

光在基底中传播的相位变化：

```math
\delta_2 = \frac{2\pi n_2 d_{sub}}{\lambda}
```

### 3.2 相位因子

```math
\phi_1 = e^{i\delta_1} = e^{i \frac{2\pi (n+ik)d}{\lambda}}
```

```math
\phi_2 = e^{i\delta_2} = e^{i \frac{2\pi n_2 d_{sub}}{\lambda}}
```

### 3.3 吸收衰减

PDMS薄膜中的吸收系数（Beer-Lambert定律）：

```math
\alpha_{PDMS} = \frac{4\pi k}{\lambda}
```

光强衰减因子：

```math
A_{abs} = e^{-\alpha_{PDMS} \cdot d} = e^{-\frac{4\pi k d}{\lambda}}
```

---

## 四、多重反射干涉

### 4.1 PDMS薄膜的有效反射系数

考虑界面0和界面1之间的多重反射：

```math
r_{film} = \frac{r_{01} + r_{12} \phi_1^2 A_{abs}^2}{1 + r_{01} r_{12} \phi_1^2 A_{abs}^2}
```

其中：
- `r₀₁` - 空气/PDMS界面反射系数
- `r₁₂` - PDMS/基底界面反射系数
- `φ₁²` - 往返一次的相位因子
- `A_{abs}²` - 往返一次的吸收衰减

### 4.2 透过PDMS薄膜的透射系数

```math
t_{film} = \frac{t_{01} t_{12} \phi_1 A_{abs}}{1 + r_{01} r_{12} \phi_1^2 A_{abs}^2}
```

---

## 五、完整系统的反射率和透射率

### 5.1 总反射系数

考虑所有界面和多重反射：

```math
r_{total} = r_{film} + \frac{|t_{film}|^2 r_{23} \phi_2^2}{1 - r_{12} r_{23} \phi_2^2}
```

第一项：PDMS薄膜的直接反射
第二项：透过薄膜、基底反射、再透回的贡献

### 5.2 总透射系数

```math
t_{total} = \frac{t_{film} t_{23} \phi_2}{1 - r_{12} r_{23} \phi_2^2}
```

### 5.3 能量反射率

```math
R = |r_{total}|^2
```

### 5.4 能量透射率

考虑折射率变化的能流密度修正：

```math
T = |t_{total}|^2 \cdot \frac{\text{Re}(n_3)}{\text{Re}(n_0)} = |t_{total}|^2 \cdot \frac{1}{1} = |t_{total}|^2
```

（空气中 `n₀ = n₃ = 1`）

---

## 六、吸收率与发射率

### 6.1 能量守恒

```math
R + T + A = 1
```

### 6.2 吸收率

```math
A = 1 - R - T
```

### 6.3 基尔霍夫定律

在热平衡时，吸收率等于发射率：

```math
\varepsilon(\lambda) = \alpha(\lambda) = A
```

---

## 七、发射率的最终表达式

### 7.1 透明基底系统

```math
\varepsilon(\lambda, d) = 1 - R(\lambda, d) - T(\lambda, d)
```

展开为：

```math
\varepsilon(\lambda, d) = 1 - |r_{total}|^2 - |t_{total}|^2
```

其中：

```math
r_{total} = \frac{r_{01} + r_{12} e^{i\frac{4\pi(n+ik)d}{\lambda}} e^{-\frac{8\pi kd}{\lambda}}}{1 + r_{01}r_{12} e^{i\frac{4\pi(n+ik)d}{\lambda}} e^{-\frac{8\pi kd}{\lambda}}} + \frac{|t_{film}|^2 r_{23} e^{i\frac{4\pi n_2 d_{sub}}{\lambda}}}{1 - r_{12}r_{23} e^{i\frac{4\pi n_2 d_{sub}}{\lambda}}}
```

```math
t_{total} = \frac{t_{01} t_{12} e^{i\frac{2\pi(n+ik)d}{\lambda}} e^{-\frac{4\pi kd}{\lambda}}}{1 + r_{01}r_{12} e^{i\frac{4\pi(n+ik)d}{\lambda}} e^{-\frac{8\pi kd}{\lambda}}} \cdot \frac{t_{23} e^{i\frac{2\pi n_2 d_{sub}}{\lambda}}}{1 - r_{12}r_{23} e^{i\frac{4\pi n_2 d_{sub}}{\lambda}}}
```

菲涅尔系数：

```math
r_{01} = \frac{1 - (n+ik)}{1 + (n+ik)}
```

```math
r_{12} = \frac{(n+ik) - n_2}{(n+ik) + n_2}
```

```math
r_{23} = \frac{n_2 - 1}{n_2 + 1}
```

```math
t_{01} = \frac{2}{1 + (n+ik)}
```

```math
t_{12} = \frac{2(n+ik)}{(n+ik) + n_2}
```

```math
t_{23} = \frac{2n_2}{n_2 + 1}
```

### 7.2 不透明基底系统

当基底不透明时，`T = 0`，发射率简化为：

```math
\varepsilon(\lambda, d) = 1 - R(\lambda, d)
```

其中：

```math
R = \left| \frac{r_{01} + r_{12} e^{i\frac{4\pi(n+ik)d}{\lambda}}}{1 + r_{01}r_{12} e^{i\frac{4\pi(n+ik)d}{\lambda}}} \right|^2
```

---

## 八、参数说明

### 8.1 输入参数

| 符号 | 含义 | 单位 | 典型值 |
|------|------|------|--------|
| `λ` | 波长 | μm | 0.4 ~ 20 |
| `d` | PDMS薄膜厚度 | μm | 0.1 ~ 100 |
| `n(λ)` | PDMS折射率 | - | 1.39 ~ 1.41 |
| `k(λ)` | PDMS消光系数 | - | 10⁻⁶ ~ 10⁻¹ |
| `n₂` | 基底折射率 | - | 1.5 (玻璃) |
| `d_sub` | 基底厚度 | μm | 100 |
| `θ` | 入射角 | rad | 0 (垂直入射) |

### 8.2 输出结果

| 符号 | 含义 | 范围 |
|------|------|------|
| `R` | 反射率 | 0 ~ 1 |
| `T` | 透射率 | 0 ~ 1 |
| `A` | 吸收率 | 0 ~ 1 |
| `ε` | 发射率 | 0 ~ 1 |

约束条件：`R + T + A = 1`，`ε = A`

---

## 九、特殊情况与近似

### 9.1 薄膜极薄时（d → 0）

```math
\delta_1 \to 0, \quad e^{i\delta_1} \to 1, \quad A_{abs} \to 1
```

```math
r_{film} \to \frac{r_{01} + r_{12}}{1 + r_{01}r_{12}}
```

干涉效应消失，退化为双界面系统。

### 9.2 无吸收时（k → 0）

```math
A_{abs} \to 1, \quad A \to 0, \quad \varepsilon \to 0
```

发射率趋于零（完全透明或反射系统）。

### 9.3 强吸收时（k 很大）

```math
A_{abs} \to 0
```

多重反射被强烈抑制：

```math
r_{film} \to r_{01}
```

```math
\varepsilon \approx 1 - |r_{01}|^2
```

发射率主要由第一界面的反射决定。

### 9.4 可见光区（k ≈ 10⁻⁶）

PDMS几乎透明，`A_{abs} ≈ 1`：

- **不透明基底**:
  ```math
  \varepsilon \approx 1 - R \approx 0.97
  ```
  （低反射导致高发射率）

- **透明基底**:
  ```math
  \varepsilon = 1 - R - T \approx 1 - 0.03 - 0.93 = 0.04
  ```
  （高透射导致低发射率）

### 9.5 红外区（k ≈ 10⁻² ~ 10⁻¹）

PDMS强吸收，无论基底透明与否：

```math
\varepsilon \approx 1 - R \approx 0.9
```

发射率主要由PDMS本身的吸收决定。

---

## 十、厚度依赖性

### 10.1 干涉周期

发射率随厚度振荡，周期为：

```math
\Delta d = \frac{\lambda}{2n}
```

当 `d = m \frac{\lambda}{4n}` (m = 1, 3, 5, ...) 时，出现干涉极值。

### 10.2 厚度对发射率的影响机制

1. **几何光程**: `δ ∝ n·d`
   - 影响干涉相位
   - 导致发射率振荡

2. **吸收累积**: `A_{abs} = e^{-αd}`
   - 厚度增加，吸收增强
   - 可见光区影响小（k小）
   - 红外区影响大（k大）

3. **综合效应**:
   - 薄膜区（d < λ/n）：干涉主导
   - 中等厚度（d ≈ λ/n）：干涉+吸收
   - 厚膜区（d >> λ/n）：吸收主导，发射率趋于稳定

---

## 十一、波长依赖性

### 11.1 发射率的光谱特性

```math
\varepsilon = \varepsilon(\lambda, d, n(\lambda), k(\lambda))
```

主要因素：
1. `n(λ)` - 影响干涉相位
2. `k(λ)` - 影响吸收强度
3. `λ` - 直接影响相位和吸收系数

### 11.2 不同波段的特征

| 波段 | λ (μm) | k | 主导机制 | 典型ε |
|------|--------|---|----------|-------|
| 可见光 | 0.4-0.8 | ~10⁻⁶ | 低反射(不透明基底)<br>高透射(透明基底) | 0.97<br>0.04 |
| 近红外 | 0.8-3 | ~10⁻⁶~10⁻⁵ | 低反射+弱吸收 | 0.95-0.98 |
| 中红外 | 3-8 | ~10⁻⁴~10⁻² | 中等吸收 | 0.90-0.97 |
| 远红外 | 8-20 | ~10⁻²~10⁻¹ | 强吸收 | 0.85-0.95 |

---

## 十二、计算流程总结

### 完整计算步骤

给定：`λ, d, n₂, d_sub`

**步骤1**: 获取光学常数
```math
n = n_{interp}(\lambda), \quad k = k_{interp}(\lambda)
```

**步骤2**: 计算菲涅尔系数
```math
r_{01}, t_{01}, r_{12}, t_{12}, r_{23}, t_{23}
```

**步骤3**: 计算相位和吸收
```math
\delta_1 = \frac{2\pi(n+ik)d}{\lambda}, \quad \delta_2 = \frac{2\pi n_2 d_{sub}}{\lambda}
```
```math
\alpha = \frac{4\pi k}{\lambda}, \quad A_{abs} = e^{-\alpha d}
```

**步骤4**: 计算薄膜反射和透射
```math
r_{film} = \frac{r_{01} + r_{12} e^{2i\delta_1} A_{abs}^2}{1 + r_{01}r_{12} e^{2i\delta_1} A_{abs}^2}
```
```math
t_{film} = \frac{t_{01}t_{12} e^{i\delta_1} A_{abs}}{1 + r_{01}r_{12} e^{2i\delta_1} A_{abs}^2}
```

**步骤5**: 计算总反射和透射
```math
r_{total} = r_{film} + \frac{|t_{film}|^2 r_{23} e^{2i\delta_2}}{1 - r_{12}r_{23} e^{2i\delta_2}}
```
```math
t_{total} = \frac{t_{film} t_{23} e^{i\delta_2}}{1 - r_{12}r_{23} e^{2i\delta_2}}
```

**步骤6**: 计算能量量
```math
R = |r_{total}|^2, \quad T = |t_{total}|^2
```

**步骤7**: 计算发射率
```math
A = 1 - R - T, \quad \varepsilon = A
```

---

## 十三、模型验证

### 13.1 物理约束

1. **能量守恒**: `R + T + A = 1` ✓
2. **非负性**: `R, T, A, ε ≥ 0` ✓
3. **上界**: `R, T, A, ε ≤ 1` ✓
4. **基尔霍夫定律**: `ε = A` ✓

### 13.2 极限情况检验

1. **d = 0**: `ε → 0` (无薄膜) ✓
2. **k = 0**: `A → 0, ε → 0` (无吸收) ✓
3. **不透明基底**: `T = 0, ε = 1 - R` ✓
4. **k → ∞**: `ε → 1 - |r₀₁|²` (强吸收) ✓

---

## 十四、参考文献与理论依据

1. **菲涅尔公式**:
   - Born, M. & Wolf, E. (1999). *Principles of Optics*. Cambridge University Press.

2. **薄膜光学**:
   - Macleod, H. A. (2010). *Thin-Film Optical Filters*. CRC Press.

3. **基尔霍夫定律**:
   - Siegel, R. & Howell, J. R. (2002). *Thermal Radiation Heat Transfer*. Taylor & Francis.

4. **传输矩阵方法**:
   - Yeh, P. (2005). *Optical Waves in Layered Media*. Wiley-Interscience.

5. **PDMS光学性质**:
   - Querry, M. R. (1987). *Optical constants of minerals and other materials*.
