## Lamarck &nbsp; &nbsp; &nbsp; 2025-10-30
#### 该文档用于复现ProteinMPNN文档中的全部示例
---

*环境 & 路径*
```bash
236 server上的环境: lmk_proteinMPNN
236 server上的路径: /data/lmk/ProteinMPNN/protein_mpnn_run.py
```

*01  sample 1: 单体设计 -- 根据输入蛋白的主链结构，生成可能折叠成该结构的氨基酸序列*
```
/data/lmk/mpnn_doc/mpnn_input文件夹下存放所有输入pdb
parsed_pdbs.jsonl通过parse_multiple_chains.py自动生成，作用是将原始的PDB文件结构信息转化成ProteinMPNN能直接理解的格式
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



##### [ProteinMPNN官方文档](https://github.com/dauparas/ProteinMPNN)






























































