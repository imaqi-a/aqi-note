<!-- 学习目标：使用matplotlib进行数据可视化 -->
```python
# stage5_visualization.py
import matplotlib.pyplot as plt
import json
from collections import defaultdict

# 设置中文字体
plt.rcParams['font.sans-serif'] = ['SimHei']  # 用来正常显示中文标签
plt.rcParams['axes.unicode_minus'] = False    # 用来正常显示负号

def load_financial_data(filename="finance_data.json"):
    """加载财务数据"""
    try:
        with open(filename, 'r', encoding='utf-8') as f:
            return json.load(f)
    except FileNotFoundError:
        print("数据文件不存在，请先运行前面的程序")
        return None

def create_expense_pie_chart(data):
    """创建支出饼图"""
    # 按类别统计支出
    category_expenses = defaultdict(float)
    for expense in data["expenses"]:
        category_expenses[expense["category"]] += expense["amount"]
    
    if not category_expenses:
        print("没有支出数据可显示")
        return
    
    # 准备饼图数据
    categories = list(category_expenses.keys())
    amounts = list(category_expenses.values())
    
    # 创建饼图
    plt.figure(figsize=(10, 8))
    plt.pie(amounts, labels=categories, autopct='%1.1f%%', startangle=90)
    plt.title('支出分类比例')
    plt.axis('equal')  # 确保饼图是圆的
    plt.tight_layout()
    plt.savefig('expense_pie_chart.png')
    plt.show()

def create_income_vs_expense_bar(data):
    """创建收入vs支出柱状图"""
    total_income = sum(item["amount"] for item in data["income"])
    total_expenses = sum(item["amount"] for item in data["expenses"])
    
    # 创建柱状图
    plt.figure(figsize=(8, 6))
    categories = ['收入', '支出']
    amounts = [total_income, total_expenses]
    colors = ['green', 'red']
    
    bars = plt.bar(categories, amounts, color=colors, alpha=0.7)
    plt.title('收入 vs 支出')
    plt.ylabel('金额 (元)')
    
    # 在柱子上显示数值
    for bar, amount in zip(bars, amounts):
        plt.text(bar.get_x() + bar.get_width()/2, bar.get_height() + 50, 
                f'{amount:.0f}元', ha='center', va='bottom')
    
    plt.tight_layout()
    plt.savefig('income_vs_expense.png')
    plt.show()

def create_monthly_trend(data):
    """创建月度趋势图（模拟数据）"""
    # 这里我们模拟6个月的数据来展示趋势
    months = ['1月', '2月', '3月', '4月', '5月', '6月']
    
    # 模拟收入数据（基于现有数据估算）
    base_income = sum(item["amount"] for item in data["income"]) or 8000
    income_trend = [base_income * (1 + i * 0.05) for i in range(6)]
    
    # 模拟支出数据
    base_expense = sum(item["amount"] for item in data["expenses"]) or 4500
    expense_trend = [base_expense * (1 + i * 0.02) for i in range(6)]
    
    # 创建趋势图
    plt.figure(figsize=(10, 6))
    plt.plot(months, income_trend, marker='o', label='收入', color='green', linewidth=2)
    plt.plot(months, expense_trend, marker='s', label='支出', color='red', linewidth=2)
    plt.title('月度收支趋势')
    plt.xlabel('月份')
    plt.ylabel('金额 (元)')
    plt.legend()
    plt.grid(True, alpha=0.3)
    plt.tight_layout()
    plt.savefig('monthly_trend.png')
    plt.show()

def create_savings_progress(data):
    """创建储蓄进度图"""
    total_income = sum(item["amount"] for item in data["income"])
    total_expenses = sum(item["amount"] for item in data["expenses"])
    savings = total_income - total_expenses
    
    # 假设储蓄目标
    savings_goal = 10000
    
    # 创建进度图
    fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12, 5))
    
    # 左侧：当前储蓄 vs 目标
    labels = ['已储蓄', '剩余目标']
    amounts = [savings, max(0, savings_goal - savings)]
    colors = ['lightblue', 'lightgray']
    
    ax1.pie(amounts, labels=labels, colors=colors, autopct='%1.1f%%', startangle=90)
    ax1.set_title('储蓄进度')
    
    # 右侧：进度条
    progress = min(savings / savings_goal, 1.0) * 100
    ax2.barh([0], [progress], color='lightgreen', alpha=0.7)
    ax2.barh([0], [100 - progress], left=[progress], color='lightgray', alpha=0.7)
    ax2.set_xlim(0, 100)
    ax2.set_yticks([])
    ax2.set_xlabel('完成百分比 (%)')
    ax2.set_title(f'储蓄目标进度: {progress:.1f}%')
    ax2.text(progress/2, 0, f'{savings:.0f}元', ha='center', va='center', fontsize=12)
    ax2.text(progress + (100-progress)/2, 0, f'{savings_goal-savings:.0f}元', 
             ha='center', va='center', fontsize=12)
    
    plt.tight_layout()
    plt.savefig('savings_progress.png')
    plt.show()

def main():
    print("=== 财务数据可视化 ===")
    
    # 加载数据
    data = load_financial_data()
    if not data:
        return
    
    # 显示基本统计
    total_income = sum(item["amount"] for item in data["income"])
    total_expenses = sum(item["amount"] for item in data["expenses"])
    balance = total_income - total_expenses
    
    print(f"数据统计:")
    print(f"- 总收入: {total_income:.2f} 元")
    print(f"- 总支出: {total_expenses:.2f} 元")
    print(f"- 当前余额: {balance:.2f} 元")
    
    # 创建可视化图表
    print("\n生成可视化图表...")
    
    if data["expenses"]:
        create_expense_pie_chart(data)
    
    create_income_vs_expense_bar(data)
    create_monthly_trend(data)
    create_savings_progress(data)
    
    print("所有图表已生成并保存为PNG文件！")

if __name__ == "__main__":
    main()
```