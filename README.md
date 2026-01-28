# 碎屑岩岩性鉴定

该脚本用于测井数据的碎屑岩岩性（Facies）识别与验证，集成多种树模型与深度模型，并进行特征增强、特征筛选、模型集成与评估输出，最终生成提交结果与可视化评估图表。

## 主要功能
- 数据清洗与稳健处理（异常值裁剪、缺失值填充）
- Bestagini 窗口 + 梯度特征增强
- 多模型训练：LightGBM / XGBoost / CatBoost / Extra Trees / Random Forest / Transformer / CNN-BiLSTM
- 静态/动态加权集成与概率校准
- 输出验证评估图表、逐模型预测结果与最终提交文件

## 数据要求
- `train.csv`：包含 `WELL`、`DEPTH`、`label` 以及各类测井曲线特征列（如 SP/GR/AC/RHOB/NPHI/RT 等）
- `validation_without_label.csv`：不含 `label`，其余列与训练集保持一致
- 测试集如包含 `ID` 或 `id` 列，将被用于生成提交文件的 `id` 字段

## 运行方式
在脚本所在目录执行：
```bash
python 碎屑岩岩性鉴定.py
```
如需自定义数据路径，修改脚本末尾：
```python
submission = identifier.train_and_predict(
    "train.csv",
    "validation_without_label.csv",
)
```

## 输出说明
运行后会在当前目录生成：
- `碎屑岩岩性鉴定_submission_fixed_leak.csv`：最终预测结果（文件名前缀由脚本名自动生成）
- `model_evaluation_outputs/`：模型评估输出目录
  - `per_model_predictions/`：各模型验证集预测结果
  - 模型对比图、箱线图、混淆矩阵等评估图表
- `selected_features_scores.csv`：特征筛选后的评分与排名

## 依赖环境
建议 Python 3.9+，核心依赖如下：
```bash
pip install numpy pandas scikit-learn scipy matplotlib seaborn torch lightgbm xgboost catboost
```
如使用 GPU，请确保已安装匹配 CUDA 版本的 PyTorch。

## 说明
- 脚本内已固定随机种子（默认 42），便于结果复现。
- 输出目录与文件名基于脚本名自动加前缀，便于多版本脚本并行实验管理。
