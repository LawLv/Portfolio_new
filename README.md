# Portfolio_new: 深度强化学习在金融投资组合管理中的应用  
*A Deep Reinforcement Learning Framework for Financial Portfolio Management*

本项目基于论文 [Jiang et al., 2017] 提出的深度强化学习方法，实现在加密货币市场中进行投资组合自动管理。通过神经网络策略和多目标优化，智能代理在考虑风险的同时追求高收益。

---

## 项目要点 (Highlights)

- 实现多种基于深度强化学习的投资组合策略  
  *Implemented CNN, RNN, and LSTM-based strategies for portfolio selection*

- 引入多目标强化学习，优化风险/收益平衡  
  *Introduced multi-objective reward functions to balance return and volatility*

- 使用 NSGA-II 遗传算法优化策略选择  
  *Used NSGA-II to find Pareto-optimal strategies*

- 多轮回测显著优于基准策略  
  *Achieved better performance than baseline methods (CRP, OLMAR)*

---

## 项目背景 (Background)

金融市场具有高波动性和不确定性，传统策略难以泛化。本项目结合强化学习和深度神经网络，在无需预设市场模型的前提下，从历史数据中自动学习投资策略。

> *This project learns portfolio policies from market data using deep RL, without explicit financial models.*

---

## 方法简介 (Methodology Overview)

### 网络结构 (Network Architecture)

- **EIIE**: 每个资产独立评估，网络共享参数  
  *Each asset is evaluated independently using a shared CNN/RNN/LSTM model*

- **PVM**: 存储历史权重向量  
  *Stores recent portfolio weights to preserve memory*

- **OSBL**: 在线随机批次训练  
  *Online Stochastic Batch Learning for time-sensitive updates*

### 奖励函数 (Reward Function)

- 考虑平均收益与下行风险  
  *Reward = α × Mean(Return) − β × Std(Drawdown)*  
  *Designed for multi-objective reinforcement learning*

### 多目标优化 (Multi-objective Optimization)

- 使用 NSGA-II 搜索帕累托最优策略集  
  *Non-dominated Sorting Genetic Algorithm II (NSGA-II) used for optimization*

---

## 使用说明 (Usage)

### 1. 数据准备 (Data Preparation)

```bash
# 默认在线下载，也可读取本地缓存数据
# Config file: net_config.json
```

### 2. 网络结构配置 (Configure Model)

编辑 `net_config.json` 配置网络类型、特征维度、窗口长度等。

```json
{
  "net_type": "cnn",
  "input_window": 50,
  "features": 3,
  "assets": 11
}
```

### 3. 模型训练 (Train Model)

```bash
python create_train_dir.py
python train.py --config net_config.json
```

支持多种种子并行训练，结果写入 `train_summary.csv`。

### 4. 回测测试 (Backtest)

```bash
python backtest.py --model model_name --config net_config.json
```

支持结果可视化。

---

## 实验结果 (Results)

| Algorithm | fAPV (Final Accumulated Portfolio Value) | SR (Sharpe Ratio) | MDD (Max Drawdown) |
|-----------|------------------------------------------|-------------------|---------------------|
| elite1    | 45.43                                    | 0.0880            | 0.2204              |
| elite2    | 44.98                                    | 0.0888            | 0.2274              |
| origin    | 44.36                                    | 0.0887            | 0.2217              |
| OLMAR     | 4.59                                     | 0.0366            | 0.6049              |
| CRP       | 1.84                                     | 0.0342            | 0.2336              |

> *Our methods outperform CRP and OLMAR in both return and risk-adjusted metrics.*

---

## 项目结构 (Project Structure)

```
Portfolio_new/
├── data/                  # 数据与缓存
├── models/                # 保存训练模型
├── configs/               # 参数配置
├── train.py               # 训练脚本
├── backtest.py            # 回测脚本
├── create_train_dir.py    # 初始化训练目录
├── utils.py               # 辅助函数
└── net_config.json        # 网络配置文件
```

---

## 后续改进方向 (Future Work)

- 替换强化学习模型（如 DDPG, PPO 等）
- 演化搜索 reward 函数参数 (α, β, k)
- 引入更多市场数据（股票、ETF）
- 加强跨周期表现（适应牛市与熊市）
- 支持在线部署与实时预测

---

## 引用与参考 (Reference)

> Jiang, Zhengyao, Dixing Xu, and Jinjun Liang.  
> *A deep reinforcement learning framework for the financial portfolio management problem.*  
> IEEE Transactions on Neural Networks and Learning Systems, 2017.

---

## 项目作者 (Authors)

陈驿来、杨可芸、潘腾  
指导老师：杨鹏  
SUSTech, March 2023
