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

## 🧪 环境配置

```bash
# Conda 环境
conda activate lmk_proteinMPNN

# 主程序路径
/data/lmk/ProteinMPNN/protein_mpnn_run.py

# 输入输出路径
输入结构: /data/lmk/mpnn_doc/mpnn_input/
输出结果: /data/lmk/mpnn_doc/mpnn_output/
```

---

## 🧱 功能目录 (Feature Overview)

> 该部分总结了仓库中每个示例脚本的功能、主要输入输出、适用场景与关键脚本文件。  
> 每个示例脚本 (`sample_X.sh`) 都可在 Linux 环境下直接运行。

| 示例编号 | 功能类型 | 主要作用 | 输入文件 | 输出文件 | 关键脚本 |
|:--:|:--|:--|:--|:--|:--|
| **01** | 🧩 简单单体设计 | 最基础的单链设计任务，熟悉输入/输出格式 | `.pdb` | `.fa`, `parsed_pdbs.jsonl` | `parse_multiple_chains.py`, `protein_mpnn_run.py` |
| **02** | 🔗 多链复合物设计 | 针对多链复合物（如抗原-抗体），只设计指定链 | `.pdb` | `.fa`, `assigned_pdbs.jsonl` | `assign_fixed_chains.py` |
| **03** | ⚡ 直接路径输入设计 | 无需预解析 `.jsonl`，可直接从 PDB 文件路径运行 | `.pdb` | `.fa` | `protein_mpnn_run.py` |
| **04-1** | 🧱 固定残基不设计 | 指定残基保持原序列不变（固定位点） | `.pdb` | `.fa`, `fixed_pdbs.jsonl` | `make_fixed_positions_dict.py` |
| **04-2** | 🧬 仅设计指定残基 | 相反操作，仅让特定残基可设计 | `.pdb` | `.fa`, `fixed_pdbs.jsonl` | `make_fixed_positions_dict.py (--specify_non_fixed)` |
| **05** | 🔁 对称性设计 | 绑定多条链中对应残基，使其协同设计（对称约束） | `.pdb` | `.fa`, `tied_pdbs.jsonl`, `fixed_pdbs.jsonl` | `make_tied_positions_dict.py` |
| **06** | 🧫 同源寡聚体设计 | 针对C2/C3/C4对称复合物的对称性序列生成 | `.pdb` | `.fa`, `tied_pdbs.jsonl` | `make_tied_positions_dict.py (--homooligomer 1)` |
| **07** | 📊 输出氨基酸概率 | 输出每个残基20种氨基酸的概率分布矩阵 | `.pdb` | `.npz`（包含概率张量） | `protein_mpnn_run.py (--unconditional_probs_only 1)` |
| **08** | 🎯 氨基酸偏置设计 | 调整氨基酸偏好，如鼓励芳香族或抑制半胱氨酸 | `.pdb` | `.fa`, `bias_pdbs.jsonl` | `make_bias_AA.py` |
