# 异构图中的边预测
这篇文章的工作是社会网络分析课程的期末大作业
## 介绍
由于图神经网络[1,2]近年来已经占据了图挖掘研究的中心阶段，人们开始关注其在异构图上的潜力。异构图由具有不同边信息的多种节点和边组成，将新颖而有效的图学习算法与复杂的实际生活场景联系在一起。边预测作为异构图的主要挑战之一，近两年来已经开发了各种模型，比如RGCN、GATNE、HetGNN、MAGNN、HGT、GCN、GAT。尽管已经开发了各种各样的新模型，但迄今为止，对它们如何真正取得进展的理解一直受到各自采用的独特数据处理和设置的限制。总之，这篇文章里的工作主要是对比各种模型在不同数据集上做边预测的效果。
## 文件描述
包括两类文件：模型文件和数据文件。
### 模型文件
包括baseline、GATNE、GNN、HetGNN、HGT、MAGNN、MAGNN_ini、RGCN
### 数据文件
包括三个数据集，Amazon、LastFM、PubMed，其数据特征为：  
| Dataset | Nodes | Nodes Types | Edges | Edges Types | Target |
| :---: | :---: | :---: | :---: | :---: | :---: |
| Amazon | 10099 | 1 | 148659 | 2 | item-item |
| LastFM | 20612 | 3 | 141521 | 3 | user-artist |
| PubMed | 63109 | 4 | 244986 | 10 | disease-disease |
#### Amazon
亚马逊是一个在线购物平台，其中包含电子类产品，它们之间有共同观看和共同购买的链接。
#### LastFM
LastFM是一个在线音乐网站，通过过滤掉只有一个链接的用户和标签对数据集进行预处理。
#### PubMed
PubMed4是一个生物医学文献图书馆。
#### 数据格式
- node.dat: 节点信息。每一行有(node_id, node_name, node_type_id, node_feature)。one-hot编码的node_features可以省略。每个节点类型接受一个连续的node_ids范围。而node_ids的顺序是按node_type_id排序的，这意味着node_type_id=0取第一个区间node_id, node_type_id=1取第二个区间node_id，等等。节点特征是用逗号分割的向量。
- link.dat: 边的信息。每一行都有(node_id_source, node_id_target, edge_type_id, edge_weight)。
- link.dat.test: 最初，这个文件是测试集链接。但为了防止数据泄漏问题，目标节点是随机替换的。
## 模型运行结果对比
### 评测指标
我们用ROC-AUC评估链接预测(ROC曲线下面积)和MRR(平均倒数比)指标。
### 模型选择
RGCN[3]、GATNE[4]、HetGNN[5]、MAGNN[6]、HGT[7]、GCN[2]、GAT[8]
|        |   Amazon   |            |   LastFM   |            |   PubMed   |            |
|:------:|:----------:|:----------:|:----------:|:----------:|:----------:|:----------:|
|        |   ROC-AUC  |     MRR    |   ROC-AUC  |     MRR    |   ROC-AUC  |     MRR    |
|  RGCN  | 86.34±0.28 | 93.92±0.16 | 57.21±0.09 | 77.68±0.18 | 78.29±0.18 | 90.26±0.24 |
|  GATNE | 77.39±0.50 | 92.04±0.36 | 66.87±0.16 | 85.93±0.63 | 63.39±0.65 | 80.05±0.22 |
| HetGNN | 77.74±0.24 | 91.79±0.03 | 62.09±0.01 | 83.56±0.14 | 73.63±0.01 | 84.00±0.04 |
|  MAGNN |      -     |      -     | 56.81±0.05 | 72.93±0.59 |      -     |      -     |
|   HGT  | 88.26±2.06 | 93.87±0.65 | 54.99±0.28 | 74.96±1.46 | 80.12±0.93 | 90.85±0.33 |
|   GCN  | 92.84±0.34 | 97.05±0.12 | 59.17±0.31 | 79.38±0.65 | 80.48±0.81 | 90.99±0.56 |
|   GAT  | 91.65±0.80 | 96.58±0.26 | 58.56±0.66 | 77.04±2.11 | 78.05±1.77 | 90.02±0.53 |
## 结论
总体上来看，GCN的效果最好。具体表现为在Amazon和PubMed数据集上，GCN的效果都最好；在LastFM数据集上，GATNE效果最好
## 参考
[1] Peter W Battaglia, Jessica B Hamrick, Victor Bapst, Alvaro Sanchez-Gonzalez, Vinicius Zambaldi, Mateusz Malinowski, Andrea Tacchetti, David Raposo, Adam Santoro, Ryan Faulkner, et al. 2018. Relational inductive biases, deep learning, and graph networks. arXiv preprint arXiv:1806.01261 (2018).  
[2] Thomas N Kipf and Max Welling. 2017. Semi-supervised classification with graph convolutional networks. In ICLR’17.  
[3] Michael Schlichtkrull, Thomas N Kipf, Peter Bloem, Rianne Van Den Berg, Ivan Titov, and Max Welling. 2018. Modeling relational data with graph convolutional networks. In ESWC’18. Springer, 593–607.  
[4] Yukuo Cen, Xu Zou, Jianwei Zhang, Hongxia Yang, Jingren Zhou, and Jie Tang. 2019. Representation learning for attributed multiplex heterogeneous network. In KDD’19. 1358–1368.  
[5] Chuxu Zhang, Dongjin Song, Chao Huang, Ananthram Swami, and Nitesh V Chawla. 2019. Heterogeneous graph neural network. In KDD’19. 793–803.  
[6] Xinyu Fu, Jiani Zhang, Ziqiao Meng, and Irwin King. 2020. MAGNN: metapath aggregated graph neural network for heterogeneous graph embedding. In WWW’20  
[7] Ziniu Hu, Yuxiao Dong, Kuansan Wang, and Yizhou Sun. 2020. Heterogeneous graph transformer. In WWW’20. 2704–2710.  
[8] Petar Veličković, Guillem Cucurull, Arantxa Casanova, Adriana Romero, Pietro Lio, and Yoshua Bengio. 2017. Graph attention networks. arXiv preprint arXiv:1710.10903 (2017).

