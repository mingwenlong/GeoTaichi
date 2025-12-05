# GeoTaichi（中文版）

![Github License](https://img.shields.io/github/license/Yihao-Shi/GeoTaichi)  ![Github stars](https://img.shields.io/github/stars/Yihao-Shi/GeoTaichi)  ![Github forks](https://img.shields.io/github/forks/Yihao-Shi/GeoTaichi)  [![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com)

[**快速开始**](#快速开始) | [**示例**](#示例) | [**论文**](https://www.researchgate.net/publication/380048019_GeoTaichi_A_Taichi-powered_high-performance_numerical_simulator_for_multiscale_geophysical_problems) | [**引用**](#引用) | [**联系**](#致谢)

## 简介

GeoTaichi 是一个基于 [Taichi](https://github.com/taichi-dev/taichi) 的数值计算工具包，用于高性能模拟多尺度、多物理的地球物理与岩土工程问题。

<p align="center">
    <img src="https://github.com/Yihao-Shi/GeoTaichi/blob/main/images/GeoTaichi.png" width="90%" height="90%" />
</p>

## 总览

GeoTaichi 集成了多种数值方法，目前包含：__离散元法（DEM）__、__材料点法（MPM）__、__材料点-离散元耦合法（MPDEM）__ 与 __有限元法（FEM）__，可覆盖岩土工程中 __土–碎石–结构相互作用__ 的分析。GeoTaichi 的主要组件如下所示：

<p align="center">
    <img src="https://github.com/Yihao-Shi/GeoTaichi/blob/main/images/main_component.png" width="50%" height="50%" />
</p>

GeoTaichi 是一个研究型项目，当前仍在 __持续开发__ 中。我们的愿景是为岩土工程与地球物理学界提供一个在 GPL-3.0 许可证下免费开源的软件，以促进相关数值研究。在 Taichi 生态中，我们希望进一步体现 Taichi 在科学计算方面的潜力。GeoTaichi 具备高并行、跨平台（支持 Windows、Linux、macOS）与跨体系结构（支持 CPU 与 GPU）的特性。

## 示例

如果你有很酷的案例，欢迎提交 [PR](https://github.com/Yihao-Shi/GeoTaichi/pulls)！

### 材料点法（MPM）
| [柱体塌落](example/mpm/ColumnCollapse/DPmaterial.py) | [溃坝](example/mpm/ColumnCollapse/NewtonianFluid.py) | [条形基础](example/mpm/Footing/StripFootingTresca.py) | [敏感黏土渐进破坏过程](example/mpm/ColumnCollapse/SoftDP.py) |
| --- | --- | --- | --- |
| ![柱体塌落](images/soil.gif) | ![溃坝](images/newtonian.gif) | ![条形基础](images/footing.gif) | ![黏土](images/clay.gif) |

### 离散元法（DEM）
| [颗粒堆积](example/dem/GranularPackings/polyLevelSet/packing_generate.py) | [螺钉与螺母](example/dem/ParticleSliding/screw_and_nut.py) | [泥石流](example/dem/DebrisFlow) |
| --- | --- | --- |
| ![颗粒堆积](images/lsdem.gif) | ![螺钉与螺母](images/screw_nut.gif) | ![泥石流](images/debris_flow.gif) |

|[转鼓](example/dem/RotatingDrums) | [三轴剪切试验](example/dem/TriaxialTest) |
| --- | --- |
| ![转鼓](images/drums.gif) | ![三轴剪切试验](images/force_chain.gif) |

### 材料点–离散元耦合（MPDEM）
| [球体冲击颗粒床](example/dempm/SphereImpact/plane_strain.py) | [颗粒柱冲击立方体颗粒](example/dempm/GranularImpact/granular_impact.py) | [盒体下沉入水](example/dempm/BoxSinking/box.py) |
| --- | --- | --- |
| ![球体冲击颗粒床](images/mpdem1.gif) | ![颗粒柱冲击立方体颗粒](images/mpdem2.gif) | ![盒体下沉入水](images/box_sinking.gif) |

## 快速开始

### 安装

#### 从源码安装（推荐）

##### Ubuntu
1. 切换到你希望的工作目录并下载 GeoTaichi 源码：
```
cd /path/to/desired/location/
git clone https://github.com/Yihao-Shi/GeoTaichi
cd GeoTaichi
```
2. 安装必要依赖：
```
# 安装 Python 与 pip
sudo apt-get install python3.8
sudo apt-get install python3-pip

# 安装 Python 依赖（建议固定版本）
bash requirements.sh
```
3. 安装 CUDA，详见 [官方安装指南](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html)
4. 设置环境变量：
```
sudo gedit ~/.bashrc
$ export PYTHONPATH="$PYTHONPATH:/path/to/desired/location/GeoTaichi"
source ~/.bashrc
```

##### Windows
1. 安装 Anaconda
2. 打开 Anaconda Prompt
3. 进入 `geotaichi_env.yml` 所在目录
4. 克隆 GeoTaichi：
```
git clone https://github.com/Yihao-Shi/GeoTaichi
```
5. 创建环境：
```
conda env create -f geotaichi_env.yml
```
6. 激活环境：
```
conda activate geotaichi
```
7. 修正环境变量（末尾路径替换为你本地的 GeoTaichi 路径）：
```
conda env config vars set PYTHONPATH=%PYTHONPATH%;.\path\to\GeoTaichi
```
8. 再次激活环境以生效：
```
conda activate geotaichi
```
9. 运行一个基准示例（柱体塌落）：
```
python DPmaterial
```
备注：示例脚本的第 3 行需要根据 GPU 的可用性进行修改。如果仅使用 CPU，请设置：
```
init('cpu')
```

#### 通过 pip 安装（简便）
```
pip install geotaichi
```

### 处理 VTU/VTS 文件

部分脚本会生成 VTS/VTU 文件，推荐使用 [ParaView](http://www.paraview.org/) 进行可视化。建议步骤如下：
1. 在 ParaView 中打开 `.vts` 或 `.vtu` 文件
2. 点击左侧的 “Apply” 按钮
3. 在 “Representation” 中选择 “Surface” 或 “Surface with Edges”
4. 在 “Coloring” 中选择变量与对应的量度（如 “Magnitude”、X 方向位移等）

### 文档

目前仅提供中文版本的 DEM 教程，详见 [文档](https://github.com/Yihao-Shi/GeoTaichi/blob/main/docs/GeoTaichi_tutorial_DEM_Chinese_version.pdf)。
用户可通过 Python 脚本设置数值参数并配置所需的仿真场景，更多 Python 脚本示例请查看 [示例文件夹](https://github.com/Yihao-Shi/GeoTaichi/tree/main/example)。

## 特性

### 离散元法（DEM）
离散元法通过逐个跟踪颗粒的运动来模拟颗粒材料，是研究颗粒介质行为的强大工具。
- 支持球形、多球与基于水平集的颗粒建模
- 为不规则形状颗粒提供统一的水平集函数构造方法
- 通过指定初始孔隙率或颗粒数量，在盒体/圆柱/球体/三棱柱中生成颗粒堆积
- 三种邻域搜索算法：蛮力搜索 / 链表单元 / 多级链表单元
- 两种速度更新方案：辛欧拉 / 速度 Verlet
- 四种接触模型：线弹性、Hertz-Mindlin、线性转动、能量守恒模型
- 支持无限平面（Plane）/ 面片（Facet，伺服墙）/ 三角片（适用于复杂边界）
- 支持球形颗粒的[周期性边界](example/dem/PeriodicBoundary)

### 材料点法（MPM）
材料点法是一种用于模拟固体、液体、气体以及任意连续介质的数值技术。与有限元等网格方法不同，MPM 有效避免了高变形下的网格缠结与对流误差等问题，是计算力学中的重要工具。
- 九类本构模型：线弹性 / Neo-Hookean / Von-Mises / 各向同性硬化塑性 /（状态相关）莫尔–库仑 / Drucker–Prager /（具黏聚力）改进 Cam-Clay / 牛顿流体 / 宾汉粘塑性流体
- 改进的速度投影技术：TPIC / APIC / MLS
- 三种应力更新方案：USF / USL / MUSL
- 三种稳定化技术：混合积分 / B-bar 方法 / F-bar 方法
- 两类平滑技术：应变平滑 / 压力平滑
- 支持 Dirichlet（固定/反射/摩擦）与 Neumann 边界条件
- 支持总拉格朗日与更新拉格朗日的显式 MPM
- 自由表面检测
- 支持导入[外部 CAD 文件](example/mpm/ExternalOBJ)

### MPDEM 耦合
- 两类接触模型：线弹性、Hertz-Mindlin、能量守恒模型（Barrier functions）
- 支持 DEM–MPM–网格 的接触，适用于复杂边界条件
- 多级邻域搜索
- 支持双向或单向耦合

### 后处理
- 支持从指定时间步重启
- 提供基于 [Taichi](https://github.com/taichi-dev/taichi) 的简易 GUI
- 仿真过程中生成 VTU（用于 [ParaView](http://www.paraview.org/)）与 NPZ（二进制）文件
- 支持力链可视化

## 开发中
- 正在开发结构化的 IGA（同几何分析）模块

## 许可证
本项目基于 GNU GPL v3 许可证发布，详情参见 [LICENSE](https://www.gnu.org/licenses/)。

## 引用
如果 GeoTaichi 对你有帮助，欢迎点亮 :star: 支持，我们会持续投入精力进行开发与维护 :grin::grin:。

如果你的工作使用了 GeoTaichi，请参考以下引文：
```latex
@article{shi2024geotaichi,
  title={GeoTaichi: A Taichi-powered high-performance numerical simulator for multiscale geophysical problems},
  author={Shi, YH and Guo, N and Yang, ZX},
  journal={Computer Physics Communications},
  volume={301},
  pages={109219},
  year={2024},
  publisher={Elsevier}
}
@article{shi2025gpu,
  title={GPU-accelerated level-set DEM for arbitrarily shaped particles with broad size distributions},
  author={Shi, YH and Guo, N and Yang, ZX},
  journal={Powder Technology},
  pages={121293},
  year={2025},
  publisher={Elsevier}
}
```

## 致谢
我们感谢所有贡献者的出色工作与开源精神。欢迎通过 [GitHub Issues](https://github.com/Yihao-Shi/GeoTaichi/issues) 提交问题或建议。

### 贡献者
<a href="https://github.com/Yihao-Shi/GeoTaichi/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=Yihao-Shi/GeoTaichi" />
</a>

### 联系我们
- 如需反馈问题或寻求帮助，请发送邮件至 <a href = "mailto:shiyh@zju.edu.cn">shiyh@zju.edu.cn</a>

## 发行说明
V0.4.0（2025 年 8 月 27 日）

- 详情请点击[这里](https://github.com/Yihao-Shi/GeoTaichi/releases/tag/GeoTaichi-v0.4)

V0.3.0（2024 年 12 月 12 日）

- 详情请点击[这里](https://github.com/Yihao-Shi/GeoTaichi/releases/tag/GeoTaichi-v0.3)

V0.2.2（2024 年 7 月 22 日）

- 修复圆与三角形相交面积计算
- 在 DEM 模块中新增 “Destroy” 与 “Reflect” 边界，示例见[此处](https://github.com/Yihao-Shi/GeoTaichi/blob/main/example/dem/SimpleChute/simple_chute.py)

V0.2（2024 年 7 月 1 日）

- 修复 DEM 与 MPM 模块中的若干问题，详见[发布说明](https://github.com/Yihao-Shi/GeoTaichi/releases/tag/GeoTaichi-v0.2)
- 新增若干高级本构模型

V0.1（2024 年 1 月 21 日）

- GeoTaichi 首次公开发布
