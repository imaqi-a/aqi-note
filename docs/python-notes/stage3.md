<!-- 学习目标：函数定义、字典、模块化编程 -->
```python
# stage3_functions_dicts.py
import json

# 财务记录数据结构
financial_data = {
    "income": [],
    "expenses": []
}

def add_income(amount, source, date):
    """添加收入记录"""
    financial_data["income"].append({
        "amount": amount,
        "source": source,
        "date": date
    })
    print(f"已记录收入: {amount}元 ({source})")

def add_expense(amount, category, description, date):
    """添加支出记录"""
    financial_data["expenses"].append({
        "amount": amount,
        "category": category,
        "description": description,
        "date": date
    })
    print(f"已记录支出: {amount}元 ({category} - {description})")

def calculate_balance():
    """计算当前余额"""
    total_income = sum(item["amount"] for item in financial_data["income"])
    total_expenses = sum(item["amount"] for item in financial_data["expenses"])
    return total_income - total_expenses

def generate_report():
    """生成财务报告"""
    balance = calculate_balance()
    
    print("\n" + "="*40)
    print("          财务报告")
    print("="*40)
    
    # 收入分析
    total_income = sum(item["amount"] for item in financial_data["income"])
    print(f"总收入: {total_income:.2f} 元")
    for income in financial_data["income"]:
        print(f"  - {income['source']}: {income['amount']}元")
    
    # 支出分析
    print(f"\n总支出: {sum(item['amount'] for item in financial_data['expenses']):.2f} 元")
    
    # 按类别统计支出
    category_totals = {}
    for expense in financial_data["expenses"]:
        category = expense["category"]
        category_totals[category] = category_totals.get(category, 0) + expense["amount"]
    
    for category, total in category_totals.items():
        percentage = (total / total_income * 100) if total_income > 0 else 0
        print(f"  - {category}: {total:.2f}元 ({percentage:.1f}%)")
    
    print(f"\n当前余额: {balance:.2f} 元")
    
    if balance > 0:
        print("财务状况: 良好 ✓")
    else:
        print("财务状况: 需要关注 ⚠")

# 主程序
def main():
    print("=== 个人财务记录系统 ===")
    
    # 示例数据
    add_income(8000, "工资", "2024-01-01")
    add_income(500, "兼职", "2024-01-15")
    add_expense(2500, "住房", "房租", "2024-01-05")
    add_expense(1200, "餐饮", "日常饮食", "2024-01-10")
    add_expense(300, "交通", "公交地铁", "2024-01-12")
    add_expense(200, "娱乐", "电影", "2024-01-20")
    
    # 生成报告
    generate_report()
    
    # 保存数据到文件
    with open("financial_data.json", "w", encoding="utf-8") as f:
        json.dump(financial_data, f, ensure_ascii=False, indent=2)
    print("\n数据已保存到 financial_data.json")

if __name__ == "__main__":
    main()
```