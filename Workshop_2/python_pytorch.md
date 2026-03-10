# Python & PyTorch 常用命令速查

> **环境说明：** WSL2 (Ubuntu 24.04) + Conda + Positron IDE

---

## Conda 环境

```bash
# 创建包含 PyTorch 的环境
conda create -n r_workshop python=3.11
conda activate r_workshop

# 安装 PyTorch（CPU 版本）
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu

# 安装 PyTorch（CUDA 版本，需要 NVIDIA GPU）
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121

# 安装常用数据科学包
pip install numpy pandas matplotlib seaborn scikit-learn jupyter
```

---

## Python 基础

```python
# 列表推导式
squares = [x**2 for x in range(10)]
evens   = [x for x in range(20) if x % 2 == 0]

# 字典推导式
d = {k: v for k, v in zip(keys, values)}

# f-string（推荐）
name = "Alice"
print(f"Hello, {name}! Age: {age:.2f}")

# 解包
a, b, *rest = [1, 2, 3, 4, 5]

# 函数
def my_func(x, y=1, *args, **kwargs):
    return x + y

# Lambda
double = lambda x: x * 2

# 类
class Animal:
    def __init__(self, name):
        self.name = name

    def speak(self):
        return f"{self.name} speaks"
```

---

## NumPy

```python
import numpy as np

# 创建数组
a = np.array([1, 2, 3])
b = np.zeros((3, 4))
c = np.ones((3, 4))
d = np.eye(3)                          # 单位矩阵
e = np.arange(0, 10, 2)               # [0, 2, 4, 6, 8]
f = np.linspace(0, 1, 100)            # 0 到 1 均匀分 100 个点
g = np.random.randn(3, 4)             # 标准正态分布

# 形状操作
a.shape
a.reshape(2, 3)
a.flatten()
np.expand_dims(a, axis=0)
np.squeeze(a)

# 数学运算
np.sum(a)
np.mean(a)
np.std(a)
np.max(a)
np.argmax(a)                           # 最大值索引
np.dot(a, b)                           # 点积
a @ b                                  # 矩阵乘法
np.concatenate([a, b], axis=0)
np.stack([a, b], axis=0)

# 索引和切片
a[0]
a[1:3]
a[:, 0]                                # 第一列
a[a > 0]                               # 布尔索引
```

---

## Pandas

```python
import pandas as pd

# 读取数据
df = pd.read_csv("data.csv")
df = pd.read_excel("data.xlsx")
df = pd.read_json("data.json")

# 写入数据
df.to_csv("output.csv", index=False)
df.to_excel("output.xlsx", index=False)

# 数据探索
df.head(10)
df.tail(10)
df.shape
df.dtypes
df.describe()
df.info()
df.isnull().sum()

# 选择数据
df["col"]                              # 单列
df[["col1", "col2"]]                   # 多列
df.loc[0:5, "col"]                     # 按标签
df.iloc[0:5, 0:3]                      # 按位置

# 过滤
df[df["age"] > 25]
df[(df["age"] > 25) & (df["city"] == "Sydney")]

# 新增列
df["bmi"] = df["weight"] / df["height"]**2

# 分组统计
df.groupby("species")["value"].mean()
df.groupby("species").agg({"value": ["mean", "std"]})

# 排序
df.sort_values("age", ascending=False)

# 缺失值
df.dropna()
df.fillna(0)
df["col"].fillna(df["col"].mean())

# 合并
pd.merge(df1, df2, on="id", how="left")
pd.concat([df1, df2], axis=0)           # 纵向拼接

# 数据透视
df.pivot_table(values="value", index="group", columns="category", aggfunc="mean")
```

---

## Matplotlib & Seaborn

```python
import matplotlib.pyplot as plt
import seaborn as sns

# 折线图
plt.plot(x, y, label="line1")
plt.xlabel("X")
plt.ylabel("Y")
plt.title("Title")
plt.legend()
plt.savefig("plot.png", dpi=300, bbox_inches="tight")
plt.show()

# 散点图
plt.scatter(x, y, c=colors, alpha=0.5)

# 直方图
plt.hist(data, bins=30)

# Seaborn（更好看）
sns.scatterplot(data=df, x="x_col", y="y_col", hue="group")
sns.boxplot(data=df, x="group", y="value")
sns.heatmap(corr_matrix, annot=True, cmap="coolwarm")
sns.pairplot(df, hue="species")

# 子图
fig, axes = plt.subplots(1, 2, figsize=(12, 5))
axes[0].plot(x, y)
axes[1].hist(data)
plt.tight_layout()
```

---

## PyTorch 基础

```python
import torch
import torch.nn as nn
import torch.optim as optim

# 查看版本和设备
print(torch.__version__)
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
print(f"Using: {device}")

# 创建 Tensor
x = torch.tensor([1.0, 2.0, 3.0])
x = torch.zeros(3, 4)
x = torch.ones(3, 4)
x = torch.randn(3, 4)
x = torch.arange(0, 10, dtype=torch.float32)

# Tensor 操作
x.shape
x.dtype
x.device
x.to(device)                           # 移到 GPU/CPU

# 数学运算
torch.matmul(a, b)                     # 矩阵乘法
a @ b                                  # 同上
torch.sum(x)
torch.mean(x)
torch.max(x)
torch.argmax(x)

# 形状操作
x.reshape(2, 3)
x.view(2, 3)
x.unsqueeze(0)                         # 增加维度
x.squeeze()                            # 去除维度
torch.cat([a, b], dim=0)              # 拼接
torch.stack([a, b], dim=0)            # 堆叠

# 与 NumPy 互转
x_np = x.numpy()                      # Tensor → NumPy（CPU only）
x_t  = torch.from_numpy(x_np)        # NumPy → Tensor

# 梯度
x = torch.randn(3, requires_grad=True)
y = x**2 + 2*x
y.sum().backward()
print(x.grad)                          # dy/dx
```

---

## PyTorch 建模

```python
import torch.nn as nn

# 定义模型
class MLP(nn.Module):
    def __init__(self, input_dim, hidden_dim, output_dim):
        super().__init__()
        self.net = nn.Sequential(
            nn.Linear(input_dim, hidden_dim),
            nn.ReLU(),
            nn.Dropout(0.2),
            nn.Linear(hidden_dim, output_dim)
        )

    def forward(self, x):
        return self.net(x)

model = MLP(input_dim=10, hidden_dim=64, output_dim=2).to(device)

# 常用 Loss 函数
criterion = nn.CrossEntropyLoss()      # 分类
criterion = nn.MSELoss()               # 回归
criterion = nn.BCEWithLogitsLoss()     # 二分类

# 优化器
optimizer = optim.Adam(model.parameters(), lr=1e-3)
optimizer = optim.SGD(model.parameters(), lr=0.01, momentum=0.9)

# 训练循环
model.train()
for epoch in range(num_epochs):
    for X_batch, y_batch in dataloader:
        X_batch, y_batch = X_batch.to(device), y_batch.to(device)

        optimizer.zero_grad()
        outputs = model(X_batch)
        loss = criterion(outputs, y_batch)
        loss.backward()
        optimizer.step()

    print(f"Epoch {epoch+1}, Loss: {loss.item():.4f}")

# 评估
model.eval()
with torch.no_grad():
    outputs = model(X_test.to(device))
    preds = torch.argmax(outputs, dim=1)
```

---

## PyTorch Dataset & DataLoader

```python
from torch.utils.data import Dataset, DataLoader

class MyDataset(Dataset):
    def __init__(self, X, y):
        self.X = torch.tensor(X, dtype=torch.float32)
        self.y = torch.tensor(y, dtype=torch.long)

    def __len__(self):
        return len(self.X)

    def __getitem__(self, idx):
        return self.X[idx], self.y[idx]

dataset = MyDataset(X, y)
dataloader = DataLoader(dataset, batch_size=32, shuffle=True, num_workers=2)
```

---

## 保存和加载模型

```python
# 保存
torch.save(model.state_dict(), "model.pth")

# 加载
model = MLP(input_dim, hidden_dim, output_dim)
model.load_state_dict(torch.load("model.pth", map_location=device))
model.eval()
```

---

## Scikit-learn 常用

```python
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.metrics import accuracy_score, classification_report, confusion_matrix

# 分割数据
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# 标准化
scaler = StandardScaler()
X_train = scaler.fit_transform(X_train)
X_test  = scaler.transform(X_test)

# 评估
accuracy_score(y_true, y_pred)
print(classification_report(y_true, y_pred))
confusion_matrix(y_true, y_pred)
```

---

## 在 Positron 里用 Python

- **Console**：交互式运行 Python 代码
- **Variables 面板**：查看所有变量和数据框
- `Ctrl + Enter`：运行当前行或选中代码
- `Ctrl + Shift + Enter`：运行整个文件
- 支持直接在 IDE 里查看 DataFrame 和图形输出
