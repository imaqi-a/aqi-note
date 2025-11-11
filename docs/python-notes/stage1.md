# Python基础语法和输入输出

## 什么是Python？
Python是一种简单易学的编程语言，就像用英语给电脑下达指令一样。我们可以用它来制作各种有用的程序，比如这个财务分析系统。

## 第一步：安装Python环境
1. 下载Python
打开浏览器，访问：https://pythonlang.cn/downloads/

点击黄色的"Download Python"按钮

下载完成后，双击安装文件

2. 安装Python（以Windows为例）
在安装界面中，一定要勾选"Add Python to PATH"

点击"Install Now"

等待安装完成

3. 验证安装是否成功
按 Win + R 键，输入 cmd，按回车

在黑色窗口中输入：python --version

如果显示类似 Python 3.x.x 的信息，说明安装成功！

4. 选择编写代码的工具
推荐使用以下任一工具：

IDLE（Python自带的，最简单）

VS Code（功能强大，免费）

记事本（最基础）

## 第二步：创建和运行你的第一个Python程序
使用记事本+命令行

打开记事本

复制粘贴我们的代码

```python
print("=== 个人财务分析系统 ===")
print("欢迎使用财务管理系统！")

# 获取用户输入
income = float(input("请输入您的月收入: "))
rent = float(input("请输入房租支出: "))
food = float(input("请输入餐饮支出: "))
transport = float(input("请输入交通支出: "))

# 计算总支出和结余
total_expenses = rent + food + transport
balance = income - total_expenses

# 显示结果
print("\n=== 财务概览 ===")
print(f"月收入: {income} 元")
print(f"总支出: {total_expenses} 元")
print(f"本月结余: {balance} 元")

# 简单的条件判断
if balance > 0:
    print("财务状况良好！")
else:
    print("注意：支出超过收入！")
```

保存为 finance_system.py（注意后缀必须是.py）

在文件所在文件夹按住 Shift + 右键，选择"在此处打开PowerShell窗口"

输入：python finance_system.py 然后回车

📖 代码逐行解释

```python
# 这一行在屏幕上显示文字
print("=== 个人财务分析系统 ===")
print("欢迎使用财务管理系统！")
```

解释：print() 就像是一个"说话"的命令，让电脑在屏幕上显示文字。

```python
# 获取用户输入
income = float(input("请输入您的月收入: "))
```
详细解释：

```input("请输入您的月收入: ")``` → 显示提示文字，等待用户输入

```float(...)``` → 把用户输入的文字转换成数字（比如把"8000"变成8000.0）

```income = ...``` → 把数字保存在一个叫"income"的盒子里，以后可以用

举个生活例子：
就像你在餐厅点餐，服务员问你要什么，你回答"一碗面"，服务员就把"一碗面"记在订单上。

```python
rent = float(input("请输入房租支出: "))
food = float(input("请输入餐饮支出: "))
transport = float(input("请输入交通支出: "))
```
解释：同样的方法，获取房租、餐饮、交通的支出金额，分别保存在 rent、food、transport 这三个"盒子"里。

```python
# 计算总支出和结余
total_expenses = rent + food + transport
balance = income - total_expenses
```
解释：

rent + food + transport → 把三个支出加起来

total_expenses = ... → 把总和保存在"total_expenses"盒子里

income - total_expenses → 用收入减去总支出

balance = ... → 把结余金额保存在"balance"盒子里

```python
# 显示结果
print("\n=== 财务概览 ===")
print(f"月收入: {income} 元")
print(f"总支出: {total_expenses} 元")
print(f"本月结余: {balance} 元")
```
解释：

```\n``` → 表示换行，让显示更整齐

```f"月收入: {income} 元"``` → 把income盒子里的数字插入到文字中显示

这样就会显示：月收入: 8000.0 元 这样的结果

```python
# 简单的条件判断
if balance > 0:
    print("财务状况良好！")
else:
    print("注意：支出超过收入！")
```
详细解释：

```if balance > 0: ``` → 如果 结余大于0

```print("财务状况良好！")``` → 就显示"财务状况良好"

```else: → ```否则（也就是结余小于或等于0）

```print("注意：支出超过收入！")``` → 就显示警告信息

生活例子：
就像检查钱包里的钱：

如果钱 > 0 → 说"钱够用"

否则 → 说"钱不够了"

🎯 运行示例
当你运行程序时，会看到这样的对话：

```text
=== 个人财务分析系统 ===
欢迎使用财务管理系统！
请输入您的月收入: 8000
请输入房租支出: 2500
请输入餐饮支出: 1500
请输入交通支出: 500

=== 财务概览 ===
月收入: 8000.0 元
总支出: 4500.0 元
本月结余: 3500.0 元
财务状况良好！
```
💡 学习要点总结
通过这个程序，你学会了：

输出信息：用 print() 显示文字

接收输入：用 input() 获取用户输入

数据类型转换：用 float() 把文字变成数字

变量：用"盒子"保存数据（income、rent等）

数学运算：加减法计算

条件判断：用 if...else 做选择

🔧 常见问题解答
Q: 如果我输入的不是数字怎么办？
A: 程序会报错，这是正常的！在后面的学习中我们会教你如何处理这种情况。

Q: 我可以修改支出类别吗？
A: 当然可以！比如把"交通支出"改成"娱乐支出"，只需要修改相应的文字即可。

Q: 为什么数字后面有".0"？
A: 因为用了 float()，它会把所有数字都当成小数。如果想显示整数，可以用 int() 代替。

🚀 下一步学习建议
恭喜你完成了第一个Python程序！接下来你可以：

尝试修改程序，增加更多的支出类别

试着改变显示的文字内容

继续学习第二阶段的教程，学习列表和循环

记住：编程就像学骑自行车，开始可能会摔倒，但多练习就会越来越熟练！💪