# R 常用命令速查

> **环境说明：** R 4.5.2，WSL2 (Ubuntu 24.04) + Positron IDE

---

## 安装与加载包

```r
# 安装包
install.packages("tidyverse")
install.packages(c("ggplot2", "dplyr", "readr"))

# 加载包
library(tidyverse)

# 查看已安装的包
installed.packages()[, "Package"]

# 更新所有包
update.packages()

# 查看包版本
packageVersion("ggplot2")
```

---

## 基本数据类型

```r
# 向量
x <- c(1, 2, 3, 4, 5)
y <- c("a", "b", "c")

# 数据框
df <- data.frame(
  name = c("Alice", "Bob"),
  age  = c(25, 30)
)

# Tibble（tidyverse 推荐）
library(tibble)
tb <- tibble(
  name = c("Alice", "Bob"),
  age  = c(25, 30)
)

# 列表
lst <- list(name = "Alice", scores = c(90, 85, 92))

# 矩阵
m <- matrix(1:9, nrow = 3, ncol = 3)
```

---

## 读写数据

```r
library(readr)

# 读取 CSV
df <- read_csv("data.csv")

# 读取 TSV
df <- read_tsv("data.tsv")

# 读取 Excel
library(readxl)
df <- read_excel("data.xlsx", sheet = 1)

# 写入 CSV
write_csv(df, "output.csv")

# 读取 RDS（R 专用格式，保留数据类型）
df <- readRDS("data.rds")
saveRDS(df, "data.rds")
```

---

## 数据探索

```r
# 快速了解数据
glimpse(df)          # 列名、类型、前几个值
head(df, 10)         # 前 10 行
tail(df, 10)         # 后 10 行
dim(df)              # 行数和列数
nrow(df)             # 行数
ncol(df)             # 列数
names(df)            # 列名
colnames(df)         # 同上

# 统计摘要
summary(df)

# 查看唯一值
unique(df$column)
n_distinct(df$column)

# 缺失值
is.na(df)
sum(is.na(df$column))
colSums(is.na(df))
```

---

## dplyr 数据操作

```r
library(dplyr)

# 筛选行
df |> filter(age > 25)
df |> filter(species == "setosa", sepal_length > 5)

# 选择列
df |> select(name, age)
df |> select(-id)              # 排除某列
df |> select(where(is.numeric))

# 新增 / 修改列
df |> mutate(bmi = weight / height^2)

# 排序
df |> arrange(age)
df |> arrange(desc(age))

# 分组统计
df |>
  summarize(
    mean_age = mean(age),
    n = n(),
    .by = species
  )

# 跨列操作
df |>
  summarize(across(where(is.numeric), list(mean = mean, sd = sd), na.rm = TRUE))

# 连接数据框
left_join(df1, df2, join_by(id))
inner_join(df1, df2, join_by(id))

# 去重
df |> distinct()
df |> distinct(species)

# 重命名列
df |> rename(new_name = old_name)

# 计数
df |> count(species)
df |> count(species, sort = TRUE)
```

---

## tidyr 数据整理

```r
library(tidyr)

# 宽转长（wide → long）
df |> pivot_longer(cols = c(col1, col2), names_to = "variable", values_to = "value")

# 长转宽（long → wide）
df |> pivot_wider(names_from = variable, values_from = value)

# 处理缺失值
df |> drop_na()                    # 删除含 NA 的行
df |> replace_na(list(col = 0))    # 替换 NA
df |> fill(col, .direction = "down")  # 向下填充
```

---

## ggplot2 可视化

```r
library(ggplot2)

# 散点图
ggplot(df, aes(x = x_col, y = y_col, color = group)) +
  geom_point() +
  labs(title = "标题", x = "X轴", y = "Y轴")

# 折线图
ggplot(df, aes(x = date, y = value)) +
  geom_line()

# 柱状图
ggplot(df, aes(x = category)) +
  geom_bar()

# 直方图
ggplot(df, aes(x = value)) +
  geom_histogram(bins = 30)

# 箱线图
ggplot(df, aes(x = group, y = value)) +
  geom_boxplot()

# 保存图片
ggsave("plot.png", width = 8, height = 6, dpi = 300)
```

---

## 字符串操作（stringr）

```r
library(stringr)

str_length("hello")              # 字符串长度
str_to_upper("hello")            # 转大写
str_to_lower("HELLO")            # 转小写
str_trim("  hello  ")            # 去除首尾空格
str_detect(x, "pattern")         # 是否包含模式
str_replace(x, "old", "new")     # 替换第一个
str_replace_all(x, "old", "new") # 替换所有
str_split(x, ",")                # 分割字符串
str_c("a", "b", sep = "-")       # 拼接：a-b
str_glue("Hello, {name}!")       # 字符串插值
```

---

## 日期时间（lubridate）

```r
library(lubridate)

today()
now()

ymd("2024-01-15")               # 字符串转日期
dmy("15-01-2024")
mdy("01/15/2024")

year(date)
month(date)
day(date)

date + days(7)                  # 加 7 天
date + months(1)                # 加 1 个月
```

---

## 函数编写

```r
# 基本函数
my_func <- function(x, y = 1) {
  result <- x + y
  return(result)
}

# 匿名函数（R 4.1+ 新语法）
double <- \(x) x * 2

# purrr 函数式编程
library(purrr)
map(list, function)              # 返回列表
map_dbl(list, function)          # 返回数值向量
map_chr(list, function)          # 返回字符向量
map2(list1, list2, function)     # 两个列表并行
```

---

## 统计分析

```r
# 基本统计
mean(x, na.rm = TRUE)
median(x, na.rm = TRUE)
sd(x, na.rm = TRUE)
var(x, na.rm = TRUE)
quantile(x, probs = c(0.25, 0.75))
cor(x, y)

# t 检验
t.test(group1, group2)
t.test(value ~ group, data = df)

# 线性回归
model <- lm(y ~ x1 + x2, data = df)
summary(model)
coef(model)
predict(model, newdata = df_new)

# 方差分析
aov_result <- aov(value ~ group, data = df)
summary(aov_result)
```

---

## 在 Positron 里用 R

- **Console**：交互式运行 R 代码
- **Environment 面板**：查看当前所有变量
- **Plots 面板**：查看图形输出
- `Ctrl + Enter`：运行当前行或选中代码
- `Ctrl + Shift + Enter`：运行整个文件
- `Alt + -`：插入 `<-`
- `Ctrl + Shift + M`：插入管道符 `|>`
