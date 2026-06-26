# Phong 光照模型实验

姓名：唐悦婷 学号：202311061051 专业：计算机科学与技术（公费师范）

基于 Taichi 实现的交互式 Phong 光照模型渲染器，包含 Blinn-Phong 升级和硬阴影功能。

## 功能特性

- **Blinn-Phong 光照模型**：实现环境光、漫反射和镜面高光三个分量的计算
- **硬阴影**：支持阴影射线检测，物体间可互相投射阴影
- **交互面板**：实时调节材质参数（Ka、Kd、Ks、Shininess）
- **三维几何体**：包含红色球体和紫色圆锥两个隐式定义的几何体

## 技术实现

### 光线投射 (Ray Casting)

- **球体相交**：使用二次方程求解光线与球体的交点
- **圆锥相交**：包含侧面和底面的完整相交测试
- **深度测试**：采用 Z-buffer 策略选择最近交点

### 光照模型

$$I = I_{ambient} + I_{diffuse} + I_{specular}$$

**环境光**：
$$I_{ambient} = K_a \times C_{light} \times C_{object}$$

**漫反射**：
$$I_{diffuse} = K_d \times \max(0, \mathbf{N} \cdot \mathbf{L}) \times C_{light} \times C_{object}$$

**镜面高光 (Blinn-Phong)**：
$$I_{specular} = K_s \times \max(0, \mathbf{N} \cdot \mathbf{H})^n \times C_{light}$$

其中 $\mathbf{H} = \frac{\mathbf{L} + \mathbf{V}}{\|\mathbf{L} + \mathbf{V}\|}$ 为半程向量。

### 硬阴影

从交点向光源发射阴影射线，检测是否被其他几何体遮挡。使用 epsilon 偏移防止阴影瑕疵。

## 场景设置

| 元素 | 参数 |
|------|------|
| 红色球体 | 圆心 (-1.2, -0.2, 0)，半径 1.2，颜色 (0.8, 0.1, 0.1) |
| 紫色圆锥 | 顶点 (1.2, 1.2, 0)，底面 y=-1.4，半径 1.2，颜色 (0.6, 0.2, 0.8) |
| 摄像机 | 位置 (0, 0, 5) |
| 点光源 | 位置 (2, 3, 4)，颜色 (1.0, 1.0, 1.0) |
| 背景色 | 深青色 (0.05, 0.15, 0.15) |

## 交互参数

| 参数 | 范围 | 默认值 | 说明 |
|------|------|--------|------|
| Ka | 0.0 ~ 1.0 | 0.2 | 环境光系数 |
| Kd | 0.0 ~ 1.0 | 0.7 | 漫反射系数 |
| Ks | 0.0 ~ 1.0 | 0.5 | 镜面高光系数 |
| Shininess | 1.0 ~ 128.0 | 32.0 | 高光指数 |

## 安装与运行

```bash
# 安装依赖
pip install taichi

# 运行程序
python phong_model.py
```

## 运行效果

程序启动后会显示一个交互式窗口，左侧为红色球体，右侧为紫色圆锥。通过右侧面板可以实时调节材质参数，观察不同光照效果。

**phong模型**  **phong模型（添加硬阴影）**

<img width="480" height="360" alt="phong" src="https://github.com/user-attachments/assets/88ff310d-dd68-4d15-9eda-8433ec762736" /> <img width="480" height="360" alt="硬阴影phong" src="https://github.com/user-attachments/assets/634e8d70-7850-4ad3-bab3-a766b62c7727" />


## Phong vs Blinn-Phong

- **Phong 模型**：使用反射向量 $\mathbf{R}$，高光区域边缘较硬，大入射角时可能出现不连续
- **Blinn-Phong 模型**：使用半程向量 $\mathbf{H}$，高光区域边缘更柔和平滑，过渡更自然

## 实验要求

1. ✅ 构建代码驱动的三维场景（球体 + 圆锥）
2. ✅ 实现光线求交与深度测试
3. ✅ 编写 Blinn-Phong 着色器
4. ✅ 完成 UI 交互面板
5. ✅ 实现硬阴影功能

## 参考资料

- Phong B T. Illumination for computer generated pictures[J]. Communications of the ACM, 1975, 18(6): 311-317.
- Blinn J F. Models of light reflection for computer synthesized pictures[C]//SIGGRAPH. 1977: 192-198.
