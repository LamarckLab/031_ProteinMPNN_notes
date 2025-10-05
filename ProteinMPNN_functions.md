## Lamarck &nbsp; &nbsp; &nbsp; 2025-10-05
#### 该文档用于复现ProteinMPNN文档中的全部示例
---

*环境 & 路径*
```bash
236 server上的环境: lmk_proteinMPNN
236 server上的路径: /data/lmk/ProteinMPNN/protein_mpnn_run.py
```

*01  sample 1: Design an unconditional monomer 无条件的单体设计*
```bash
/data/lmk/RFdiffusion/scripts/run_inference.py 'contigmap.contigs=[150-150]' inference.output_prefix=outputs_pdb/output inference.num_designs=3
# 设计一个 150aa 的单体蛋白骨架，不附加其他结构或功能限制
```

*02  sample 2: Motif Scaffolding 蛋白基序的支架设计（在蛋白片段两端延伸骨架）*
```bash
/data/lmk/RFdiffusion/scripts/run_inference.py inference.output_prefix=outputs_pdb/output inference.input_pdb=input.pdb 'contigmap.contigs=[10-20/A12-47/10-20]' inference.num_designs=3
# 从输入文件中截取A链的12-47片段，作为motif，在两端分别延伸10-20aa的骨架

/data/lmk/RFdiffusion/scripts/run_inference.py inference.output_prefix=outputs_pdb/output inference.input_pdb=input.pdb 'contigmap.contigs=[5-15/A10-25/30-40/0 B1-150]' inference.num_designs=3
# 若输入pdb中有两条链，把a链的部分结构作为motif，保留b链1-150位, /0表示链断开

/data/lmk/RFdiffusion/scripts/run_inference.py inference.output_prefix=outputs_pdb/output inference.input_pdb=input.pdb 'contigmap.contigs=[A73-268/0 B72-268/0 C44-71/0 1-2/C74-258/0]' 'ppi.hotspot_res=[B199]' inference.num_designs=20 denoiser.noise_scale_ca=0 denoiser.noise_scale_frame=0
# 保留AB两条链，延伸C链74位点这一端，往hotspot的位置
```

*03  sample 3: Partial diffusion 部分扩散*
```bash
/data/lmk/RFdiffusion/scripts/run_inference.py inference.output_prefix=outputs_pdb/output inference.input_pdb=input.pdb 'contigmap.contigs=[150-150]' inference.num_designs=10 diffuser.partial_T=10
# 对输入的pdb添加噪声并扩散，从而实现结构上一定程度上的扰动和“变异”
# diffuser.partial_T 代表加噪的步数，加噪越多，输出和输入差别越大，加噪越少，输出和输入越相似
```

*04  sample 4: Binder design 结合物设计*
```bash
/data/lmk/RFdiffusion/scripts/run_inference.py inference.output_prefix=outputs_pdb/output inference.input_pdb=input.pdb 'contigmap.contigs=[A1-150/0 70-100]' 'ppi.hotspot_res=[A15,A11,A8]' inference.num_designs=10 denoiser.noise_scale_ca=0 denoiser.noise_scale_frame=0
# 'contigmap.contigs=[A1-150/0 70-100]' 表示保留输入PDB中a链的1-150残基，设计一条70-100aa的binder，期望结合在'ppi.hotspot_res=[A15,A11,A8]'
```

*05  sample 5: Symmetric Motif Scaffolding 对称性蛋白基序的支架设计*
```bash

```

##### [RFdiffusion官方文档](https://github.com/RosettaCommons/RFdiffusion)















































