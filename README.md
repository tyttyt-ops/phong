# Blinn-Phong 光照模型与硬阴影实现

## 项目概述

姓名：唐悦婷 学号：202311061051 专业：计算机科学与技术（公费师范）

本项目使用 Taichi 图形库实现了基于 Blinn-Phong 光照模型的 3D 渲染效果，并添加了硬阴影功能。项目展示了如何在光线追踪框架中实现高级光照效果，包括环境光、漫反射、高光反射以及物体之间的阴影遮挡。

## 功能特性

- **Blinn-Phong 光照模型**：使用半程向量计算高光，实现更真实的光照效果
- **硬阴影 (Hard Shadow)**：通过暗影射线检测实现物体之间的阴影遮挡
- **交互式材质参数调整**：可实时调整环境光、漫反射、高光反射和 shininess 参数
- **多种几何体支持**：实现了球体和圆锥体的光线相交检测
- **实时渲染**：利用 Taichi 的并行计算能力实现高效渲染

## 技术实现

### 1. Blinn-Phong 模型

Blinn-Phong 模型是对传统 Phong 模型的改进，通过计算半程向量  athbf{H}  来替代反射向量，提高了高光计算的效率和视觉效果：

```python
# 计算半程向量 HH = normalize(L + V)
spec = ti.max(0.0, N.dot(H)) ** shininess[None]
specular = Ks[None] * spec * light_color
```

### 2. 硬阴影实现

通过在交点处向光源方向发射暗影射线，检测是否有其他物体遮挡光源：

```python
# 硬阴影计算
in_shadow = False
shadow_ray_dir = L
shadow_ray_orig = p + N * 1e-3  # 避免自遮挡
shadow_t_max = (light_pos - p).norm()

# 检查阴影射线是否与场景物体相交
# ...

if not in_shadow:
    # 计算完整光照
else:
    # 只计算环境光
    color = ambient
```

### 3. 几何体相交检测

实现了球体和圆锥体的光线相交检测函数：
- `intersect_sphere()`：球体相交检测
- `intersect_cone()`：圆锥体相交检测

## 安装与运行

### 环境要求

- Python 3.7+
- Taichi 1.7.4+

### 安装依赖

```bash
pip install taichi
```

### 运行程序

```bash
python phg.py
```

## 交互操作

程序运行后会打开一个窗口，右侧有材质参数控制面板：

- **Ka (Ambient)**：环境光强度
- **Kd (Diffuse)**：漫反射强度
- **Ks (Specular)**：高光反射强度
- **N (Shininess)**：高光锐利度

通过调整这些参数，可以观察不同光照效果。

## 场景描述

场景中包含两个几何体：
- **红色球体**：位于左侧，半径 1.2
- **紫色圆锥体**：位于右侧，顶点在 (1.2, 1.2, 0.0)，底面在 y=-1.4，底面半径 1.2

光源位置：(2.0, 3.0, 4.0)
观察点位置：(0.0, 0.0, 5.0)

## Blinn-Phong vs Phong 模型

### 视觉表现差异

1. **高光区域边缘**：
   - Phong 模型：在大入射角时，高光区域边缘会出现突然的明暗变化
   - Blinn-Phong 模型：高光区域边缘过渡更平滑，更符合真实物理光照

2. **计算效率**：
   - Blinn-Phong 模型避免了反射向量的计算，计算效率更高
   - 半程向量的计算比反射向量更简单

3. **高光分布**：
   - Blinn-Phong 模型的高光分布更集中，更接近真实金属材质的表现

## 代码结构

- `phg.py`：主程序文件，包含所有渲染逻辑
  - `normalize()`：向量归一化函数
  - `reflect()`：向量反射计算函数
  - `intersect_sphere()`：球体相交检测
  - `intersect_cone()`：圆锥体相交检测
  - `render()`：主渲染函数
  - `main()`：程序入口，处理窗口和交互

## 扩展建议

1. **软阴影**：实现 PCF (Percentage Closer Filtering) 软阴影
2. **多光源**：支持多个光源的光照计算
3. **材质系统**：实现更复杂的材质系统，如金属、 dielectric 等
4. **抗锯齿**：实现 MSAA 或其他抗锯齿技术
5. **纹理映射**：添加纹理映射功能

## 许可证

本项目仅供学习和教育目的使用。

## 作者

Created for computer graphics learning and practice.
