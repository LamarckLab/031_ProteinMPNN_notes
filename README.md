# 031_ProteinMPNN_notes

<h1 align="center">🧬 ProteinMPNN 全功能复现与实战指南</h1>

<p align="center">
  <i>完整复现并注释 ProteinMPNN 官方文档的全部示例脚本</i>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.8+-blue?logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/Framework-PyTorch-orange?logo=pytorch" />
  <img src="https://img.shields.io/badge/Platform-Linux-lightgrey?logo=linux" />
  <img src="https://img.shields.io/badge/Status-Complete-brightgreen" />
</p>

---

## 🧭 简介

本仓库由 **Lamarck** 编写，  
目标是 **完全复现 ProteinMPNN 官方文档** 中的所有示例脚本，  
并为科研人员提供一份可直接运行的、带有中文注释的 **使用指北**。

> 💡 通过阅读与运行本仓库，你可以快速理解 ProteinMPNN 的结构、输入输出格式、  
> 各类设计任务（多链复合物、同源寡聚体、残基约束、氨基酸偏置等）的操作逻辑。

---

## 🧪 环境配置

```bash
# Conda 环境
conda activate lmk_proteinMPNN

# 主程序路径
/data/lmk/ProteinMPNN/protein_mpnn_run.py

# 输入输出路径
输入结构: /data/lmk/mpnn_doc/mpnn_input/
输出结果: /data/lmk/mpnn_doc/mpnn_output/
