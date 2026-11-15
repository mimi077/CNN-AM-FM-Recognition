# 基于 2D CNN 的 AM/FM 调制方式识别系统

一个能"看懂"无线电信号的 AI 系统——自动识别信号是 AM（调幅）还是 FM（调频）调制，并提供完整的信号调制、加噪、解调、频谱图可视化交互演示。

## 功能特性

- **信号调制演示**：直观展示 AM 调幅与 FM 调频的波形差异
- **信道噪声模拟**：可调信噪比（SNR），观察噪声对信号的影响
- **信号解调还原**：AM 包络检波、FM 鉴频，还原原始消息信号
- **时频分析**：1D 时域信号 → 2D 频谱图（STFT 短时傅里叶变换）
- **AI 智能识别**：2D CNN 模型自动判断调制方式，输出置信度
- **交互式 Web 界面**：滑块调参、一键生成、实时可视化

## 项目结构

```
├── AM_FM_Modulation_Recognition_Paper.ipynb  # 论文 Notebook（含理论推导与实验）
├── AM_FM_Modulation_Recognition_Paper.pdf     # 论文 PDF 版
├── AM_FM_Modulation_Recognition_Paper.html    # 论文 HTML 版
├── AM_FM_Modulation_Recognition.pptx          # 演示文稿
├── start.bat                                  # Web 应用一键启动脚本（Windows）
├── app.py                                     # Flask 后端（信号处理 + CNN 推理）
├── introduction.txt                           # 项目通俗介绍
└── templates/
    └── index.html                             # Web 前端界面
```

## 快速开始

### 方式一：Web 应用（推荐，双击即用）

1. 双击 `start.bat`
2. 等待依赖安装与模型训练（首次约 1-3 分钟）
3. 浏览器自动打开 `http://127.0.0.1:5000`
4. 拖动滑块调整参数，点击按钮生成信号并测试 AI 识别

### 方式二：命令行启动

```bash
pip install numpy matplotlib scipy scikit-learn flask
python app.py
```

然后浏览器访问 `http://127.0.0.1:5000`。

### 方式三：Jupyter Notebook（论文版）

```bash
pip install numpy matplotlib scipy scikit-learn jupyter notebook seaborn torch
jupyter notebook
```

打开 `AM_FM_Modulation_Recognition_Paper.ipynb`，按 `Shift + Enter` 逐单元格运行。

## 模型说明

本系统核心为 **2D CNN（二维卷积神经网络）**，将 1D 时域信号通过 STFT 转换为 2D 频谱图后输入网络分类。

| 项目 | 说明 |
|------|------|
| 输入 | 频谱图（形状：1 × F × T） |
| 网络 | 4 层卷积（32→64→128→256）+ 自适应池化 + 全连接 |
| 输出 | AM / FM 二分类 + 置信度 |
| 训练数据 | 2000 样本（AM/FM 各 1000，随机载波频率、消息频率、SNR） |
| 训练轮数 | 8 epochs |
| 硬件 | 自动检测 CUDA GPU，无 GPU 则回退 CPU |

**SVM 回退机制**：若环境未安装 PyTorch，系统自动回退到基于 6 维统计特征的 SVM 分类器，保证基本功能可用。如需启用 CNN，请安装 PyTorch：

```bash
pip install torch
```

## 技术栈

- **后端**：Python 3.8+、Flask
- **信号处理**：NumPy、SciPy（hilbert 变换、butter 滤波、spectrogram）
- **深度学习**：PyTorch（2D CNN）
- **机器学习**：scikit-learn（SVM 回退、数据划分）
- **可视化**：Matplotlib（后端绘图转 base64）、原生 HTML/CSS/JS（前端）

## 论文 Notebook 内容

1. 引言 — 研究背景与目标
2. 理论基础 — AM/FM 调制与解调原理
3. 信号生成与调制 — 代码实现与可视化
4. 信道噪声 — AWGN 高斯白噪声模型
5. 频谱图转换 — STFT 时频分析
6. 2D CNN 模型设计 — 网络架构与参数
7. 数据集生成与模型训练
8. 模型评估 — 混淆矩阵
9. 不同 SNR 下的性能分析
10. 实时演示 — 随机信号识别
11. 结论与未来工作
12. 参考文献

## 作者信息

- **Author**: mimi
- **Date**: 2026-05-28
- **Email**: 3322346701@qq.com
- **GitHub**: https://github.com/mimi077/CNN-AM-FM-Recognition
