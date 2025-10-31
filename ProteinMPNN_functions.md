## Lamarck &nbsp; &nbsp; &nbsp; 2025-10-30
#### 该文档用于复现ProteinMPNN文档中的全部示例
---

*环境 & 路径*
```bash
236 server上的环境: lmk_proteinMPNN
236 server上的路径: /data/lmk/ProteinMPNN/protein_mpnn_run.py
```

*01  sample 1: 简单单体设计 -- 最基础的入门脚本，只设计单条链，适合熟悉 ProteinMPNN 的输入输出格式。*
```
/data/lmk/mpnn_doc/mpnn_input 文件夹下存放所有输入pdb
parsed_pdbs.jsonl通过parse_multiple_chains.py生成，作用是将原始的PDB文件结构信息转化成ProteinMPNN能直接理解的格式，记录一个输入结构的解析结果（链 ID、残基编号、坐标/掩码等元数据）
```
```bash
folder_with_pdbs="/data/lmk/mpnn_doc/mpnn_input/"
output_dir="/data/lmk/mpnn_doc/mpnn_output"
path_for_parsed_chains=$output_dir"/parsed_pdbs.jsonl"

python /data/lmk/ProteinMPNN/helper_scripts/parse_multiple_chains.py --input_path=$folder_with_pdbs --output_path=$path_for_parsed_chains

python /data/lmk/ProteinMPNN/protein_mpnn_run.py \
        --jsonl_path $path_for_parsed_chains \
        --out_folder $output_dir \
        --num_seq_per_target 2 \
        --sampling_temp "0.1" \
        --seed 37 \
        --batch_size 1
```
```bash
bash sample_1.sh
```

*02  sample 2: 多链复合物设计 -- 用于有多个蛋白链的复合物（如抗原-抗体、受体-配体），可选择固定部分链，只设计特定链。*
```
folder_with_pdbs="/data/lmk/mpnn_doc/mpnn_input 文件夹下存放所有输入pdb
parsed_pdbs.jsonl 记录一个输入结构的解析结果（链 ID、残基编号、坐标/掩码等元数据）
assigned_pdbs.jsonl 在解析基础上再写入" 哪些链设计、哪些链固定" 的分配信息
chains_to_design="A B" 表示把链A和B作为待设计链，其余链自动视作固定链
```
```bash
folder_with_pdbs="/data/lmk/mpnn_doc/mpnn_input/"
output_dir="/data/lmk/mpnn_doc/mpnn_output"
path_for_parsed_chains=$output_dir"/parsed_pdbs.jsonl"
path_for_assigned_chains=$output_dir"/assigned_pdbs.jsonl"
chains_to_design="A B"

python /data/lmk/ProteinMPNN/helper_scripts/parse_multiple_chains.py --input_path=$folder_with_pdbs --output_path=$path_for_parsed_chains

python /data/lmk/ProteinMPNN/helper_scripts/assign_fixed_chains.py --input_path=$path_for_parsed_chains --output_path=$path_for_assigned_chains --chain_list "$chains_to_design"

python /data/lmk/ProteinMPNN/protein_mpnn_run.py \
        --jsonl_path $path_for_parsed_chains \
        --chain_id_jsonl $path_for_assigned_chains \
        --out_folder $output_dir \
        --num_seq_per_target 2 \
        --sampling_temp "0.1" \
        --seed 37 \
        --batch_size 1
```
```bash
bash sample_2.sh
```

*03  sample 3: 直接从 PDB 路径读取结构设计 -- 不需要先解析成 .jsonl 文件，可以直接用 PDB 文件路径运行，方便快速测试单个 PDB 文件。*
```
/data/lmk/mpnn_doc/mpnn_input/3HTN.pdb 是需要设计的pdb路径
```
```bash
path_to_PDB="/data/lmk/mpnn_doc/mpnn_input/3HTN.pdb"
output_dir="/data/lmk/mpnn_doc/mpnn_output"

chains_to_design="A B"

python /data/lmk/ProteinMPNN/protein_mpnn_run.py \
        --pdb_path $path_to_PDB \
        --pdb_path_chains "$chains_to_design" \
        --out_folder $output_dir \
        --num_seq_per_target 2 \
        --sampling_temp "0.1" \
        --seed 37 \
        --batch_size 1
```
```bash
bash sample_3.sh
```

*04  sample 4-1: 指定哪些残基不参与设计 -- 部分氨基酸保持原序列不变，只在其他位置重新设计,如固定活性位点、金属配位残基、抗原表位等。*
```
folder_with_pdbs="/data/lmk/mpnn_doc/mpnn_input 文件夹下存放所有输入pdb
parsed_pdbs.jsonl 记录一个输入结构的解析结果（链 ID、残基编号、坐标/掩码等元数据）
assigned_pdbs.jsonl 在解析基础上再写入" 哪些链设计、哪些链固定" 的分配信息
fixed_pdbs.jsonl 指定哪些残基位置需要固定（即不被设计）
```
```bash
folder_with_pdbs="/data/lmk/mpnn_doc/mpnn_input/"
output_dir="/data/lmk/mpnn_doc/mpnn_output"

path_for_parsed_chains=$output_dir"/parsed_pdbs.jsonl"
path_for_assigned_chains=$output_dir"/assigned_pdbs.jsonl"
path_for_fixed_positions=$output_dir"/fixed_pdbs.jsonl"

chains_to_design="A C"

#这里的序号是严格的残基排序，而不是pdb中的残基index
fixed_positions="1 2 3 4 5 6 7 8 23 25, 10 11 12 13 14 15 16 17 18 19 20 40"

python /data/lmk/ProteinMPNN/helper_scripts/parse_multiple_chains.py --input_path=$folder_with_pdbs --output_path=$path_for_parsed_chains

python /data/lmk/ProteinMPNN/helper_scripts/assign_fixed_chains.py --input_path=$path_for_parsed_chains --output_path=$path_for_assigned_chains --chain_list "$chains_to_design"

python /data/lmk/ProteinMPNN/helper_scripts/make_fixed_positions_dict.py --input_path=$path_for_parsed_chains --output_path=$path_for_fixed_positions --chain_list "$chains_to_design" --position_list "$fixed_positions"

python /data/lmk/ProteinMPNN/protein_mpnn_run.py \
        --jsonl_path $path_for_parsed_chains \
        --chain_id_jsonl $path_for_assigned_chains \
        --fixed_positions_jsonl $path_for_fixed_positions \
        --out_folder $output_dir \
        --num_seq_per_target 2 \
        --sampling_temp "0.1" \
        --seed 37 \
        --batch_size 1
```
```bash
bash sample_4_1.sh
```

*05  sample 4-2: 指定哪些残基参与设计 -- 与上一个相反，这次你指定哪些残基可以被设计，其余都保持固定，常用于探索性突变设计。*
```
folder_with_pdbs="/data/lmk/mpnn_doc/mpnn_input 文件夹下存放所有输入pdb
parsed_pdbs.jsonl 记录一个输入结构的解析结果（链 ID、残基编号、坐标/掩码等元数据）
assigned_pdbs.jsonl 在解析基础上再写入" 哪些链设计、哪些链固定" 的分配信息
fixed_pdbs.jsonl 指定哪些残基位置需要固定（即不被设计）
```
```bash
folder_with_pdbs="/data/lmk/mpnn_doc/mpnn_input/"
output_dir="/data/lmk/mpnn_doc/mpnn_output"

path_for_parsed_chains=$output_dir"/parsed_pdbs.jsonl"
path_for_assigned_chains=$output_dir"/assigned_pdbs.jsonl"
path_for_fixed_positions=$output_dir"/fixed_pdbs.jsonl"

chains_to_design="A C"

design_only_positions="1 2 3 4 5 6 7 8 9 10, 3 4 5 6 7 8" 

python /data/lmk/ProteinMPNN/helper_scripts/parse_multiple_chains.py --input_path=$folder_with_pdbs --output_path=$path_for_parsed_chains
python /data/lmk/ProteinMPNN/helper_scripts/assign_fixed_chains.py --input_path=$path_for_parsed_chains --output_path=$path_for_assigned_chains --chain_list "$chains_to_design"
python /data/lmk/ProteinMPNN/helper_scripts/make_fixed_positions_dict.py --input_path=$path_for_parsed_chains --output_path=$path_for_fixed_positions --chain_list "$chains_to_design" --position_list "$design_only_positions" --specify_non_fixed

python /data/lmk/ProteinMPNN/protein_mpnn_run.py \
        --jsonl_path $path_for_parsed_chains \
        --chain_id_jsonl $path_for_assigned_chains \
        --fixed_positions_jsonl $path_for_fixed_positions \
        --out_folder $output_dir \
        --num_seq_per_target 2 \
        --sampling_temp "0.1" \
        --seed 37 \
        --batch_size 1
```
```bash
bash sample_4_2.sh
```

*06  sample 5: 对称性设计 -- 让多个位置共享同一氨基酸类型，用于二聚体、三聚体等对称性结构设计。*
```

```
```bash

```
```bash

```

##### [ProteinMPNN官方文档](https://github.com/dauparas/ProteinMPNN)































































































