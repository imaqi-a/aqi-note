# 成品
```python

import json
import os
from datetime import datetime, timedelta
import matplotlib.pyplot as plt
from collections import defaultdict

plt.rcParams['font.sans-serif'] = ['SimHei']
plt.rcParams['axes.unicode_minus'] = False

class PersonalFinanceSystem:
    def __init__(self, data_file="personal_finance.json"):
        self.data_file = data_file
        self.data = self.load_data()
    
    def load_data(self):
        """加载数据"""
        if os.path.exists(self.data_file):
            try:
                with open(self.data_file, 'r', encoding='utf-8') as f:
                    return json.load(f)
            except Exception as e:
                print(f"加载数据失败: {e}")
        
        # 返回默认数据结构
        return {
            "transactions": [],
            "categories": {
                "income": ["工资", "兼职", "投资", "奖金", "其他收入"],
                "expense": ["住房", "餐饮", "交通", "娱乐", "购物", "医疗", "教育", "其他支出"]
            },
            "budgets": {},
            "savings_goal": 10000,
            "created_date": datetime.now().isoformat()
        }
    
    def save_data(self):
        """保存数据"""
        try:
            with open(self.data_file, 'w', encoding='utf-8') as f:
                json.dump(self.data, f, ensure_ascii=False, indent=2)
            return True
        except Exception as e:
            print(f"保存数据失败: {e}")
            return False
    
    def add_transaction(self, amount, category, description, transaction_type):
        """添加交易记录"""
        transaction = {
            "id": len(self.data["transactions"]) + 1,
            "amount": float(amount),
            "category": category,
            "description": description,
            "type": transaction_type,
            "date": datetime.now().isoformat()
        }
        
        self.data["transactions"].append(transaction)
        if self.save_data():
            print(f"✓ 成功添加{transaction_type}记录")
            return True
        return False
    
    def get_financial_summary(self):
        """获取财务摘要"""
        total_income = sum(t["amount"] for t in self.data["transactions"] if t["type"] == "income")
        total_expense = sum(t["amount"] for t in self.data["transactions"] if t["type"] == "expense")
        balance = total_income - total_expense
        
        return {
            "total_income": total_income,
            "total_expense": total_expense,
            "balance": balance,
            "savings_rate": (balance / total_income * 100) if total_income > 0 else 0
        }
    
    def analyze_spending_by_category(self):
        """按类别分析支出"""
        category_spending = defaultdict(float)
        for transaction in self.data["transactions"]:
            if transaction["type"] == "expense":
                category_spending[transaction["category"]] += transaction["amount"]
        
        return dict(category_spending)
    
    def generate_visual_report(self):
        """生成可视化报告"""
        summary = self.get_financial_summary()
        spending_by_category = self.analyze_spending_by_category()
        
        # 创建多个子图
        fig, axes = plt.subplots(2, 2, figsize=(15, 12))
        fig.suptitle('个人财务分析报告', fontsize=16, fontweight='bold')
        
        # 1. 收支对比饼图
        if summary["total_income"] > 0:
            income_vs_expense = [summary["total_income"], summary["total_expense"]]
            axes[0,0].pie(income_vs_expense, 
                         labels=['收入', '支出'], 
                         autopct='%1.1f%%', 
                         colors=['#4CAF50', '#F44336'])
            axes[0,0].set_title('收入 vs 支出比例')
        
        # 2. 支出分类饼图
        if spending_by_category:
            categories = list(spending_by_category.keys())
            amounts = list(spending_by_category.values())
            axes[0,1].pie(amounts, labels=categories, autopct='%1.1f%%')
            axes[0,1].set_title('支出分类')
        
        # 3. 财务概览柱状图
        metrics = ['总收入', '总支出', '余额']
        values = [summary["total_income"], summary["total_expense"], summary["balance"]]
        colors = ['green', 'red', 'blue']
        bars = axes[1,0].bar(metrics, values, color=colors, alpha=0.7)
        axes[1,0].set_title('财务概览')
        axes[1,0].set_ylabel('金额 (元)')
        
        # 在柱子上添加数值标签
        for bar, value in zip(bars, values):
            axes[1,0].text(bar.get_x() + bar.get_width()/2, bar.get_height() + 50,
                          f'{value:.0f}元', ha='center', va='bottom')
        
        # 4. 储蓄进度
        savings_goal = self.data.get("savings_goal", 10000)
        current_savings = max(0, summary["balance"])
        progress = min(current_savings / savings_goal * 100, 100)
        
        axes[1,1].barh(['储蓄进度'], [progress], color='lightgreen')
        axes[1,1].set_xlim(0, 100)
        axes[1,1].set_xlabel('完成百分比 (%)')
        axes[1,1].set_title(f'储蓄目标: {savings_goal}元\n当前进度: {progress:.1f}%')
        axes[1,1].text(progress/2, 0, f'{current_savings:.0f}元', 
                      ha='center', va='center', fontweight='bold')
        
        plt.tight_layout()
        plt.savefig('financial_analysis_report.png', dpi=300, bbox_inches='tight')
        plt.show()
    
    def display_menu(self):
        """显示主菜单"""
        print("\n" + "="*50)
        print("           个人财务分析系统")
        print("="*50)
        print("1. 添加收入记录")
        print("2. 添加支出记录")
        print("3. 查看财务摘要")
        print("4. 查看支出分类")
        print("5. 生成可视化报告")
        print("6. 查看所有交易")
        print("7. 退出系统")
        print("="*50)
    
    def run(self):
        """运行系统"""
        print("欢迎使用个人财务分析系统！")
        
        while True:
            self.display_menu()
            choice = input("请选择操作 (1-7): ").strip()
            
            if choice == '1':
                self.add_income()
            elif choice == '2':
                self.add_expense()
            elif choice == '3':
                self.show_summary()
            elif choice == '4':
                self.show_spending_by_category()
            elif choice == '5':
                self.generate_visual_report()
            elif choice == '6':
                self.show_all_transactions()
            elif choice == '7':
                print("感谢使用个人财务分析系统！")
                break
            else:
                print("无效选择，请重新输入！")
    
    def add_income(self):
        """添加收入"""
        print("\n--- 添加收入记录 ---")
        amount = input("收入金额: ")
        print("收入类别:")
        for i, category in enumerate(self.data["categories"]["income"], 1):
            print(f"  {i}. {category}")
        
        cat_choice = input("选择类别编号: ")
        try:
            category = self.data["categories"]["income"][int(cat_choice)-1]
        except (ValueError, IndexError):
            category = input("输入自定义类别: ")
        
        description = input("收入说明: ")
        
        if self.add_transaction(amount, category, description, "income"):
            print("✓ 收入记录添加成功！")
    
    def add_expense(self):
        """添加支出"""
        print("\n--- 添加支出记录 ---")
        amount = input("支出金额: ")
        print("支出类别:")
        for i, category in enumerate(self.data["categories"]["expense"], 1):
            print(f"  {i}. {category}")
        
        cat_choice = input("选择类别编号: ")
        try:
            category = self.data["categories"]["expense"][int(cat_choice)-1]
        except (ValueError, IndexError):
            category = input("输入自定义类别: ")
        
        description = input("支出说明: ")
        
        if self.add_transaction(amount, category, description, "expense"):
            print("✓ 支出记录添加成功！")
    
    def show_summary(self):
        """显示财务摘要"""
        summary = self.get_financial_summary()
        
        print("\n" + "="*40)
        print("          财务摘要")
        print("="*40)
        print(f"总收入:   {summary['total_income']:>10.2f} 元")
        print(f"总支出:   {summary['total_expense']:>10.2f} 元")
        print(f"当前余额: {summary['balance']:>10.2f} 元")
        print(f"储蓄率:   {summary['savings_rate']:>9.1f} %")
        print("="*40)
        
        if summary['balance'] > 0:
            print("财务状况: 良好 ✓")
        else:
            print("财务状况: 需要关注 ⚠")
    
    def show_spending_by_category(self):
        """显示支出分类"""
        spending = self.analyze_spending_by_category()
        
        if not spending:
            print("暂无支出记录")
            return
        
        print("\n--- 支出分类统计 ---")
        total_spending = sum(spending.values())
        for category, amount in sorted(spending.items(), key=lambda x: x[1], reverse=True):
            percentage = (amount / total_spending * 100) if total_spending > 0 else 0
            print(f"{category:<8}: {amount:>8.2f} 元 ({percentage:>5.1f}%)")
    
    def show_all_transactions(self):
        """显示所有交易"""
        if not self.data["transactions"]:
            print("暂无交易记录")
            return
        
        print("\n--- 所有交易记录 ---")
        for transaction in self.data["transactions"]:
            date = datetime.fromisoformat(transaction["date"]).strftime("%Y-%m-%d")
            type_icon = "↑" if transaction["type"] == "income" else "↓"
            color = "32" if transaction["type"] == "income" else "31"  # 绿色/红色
            print(f"\033[{color}m{type_icon} [{date}] {transaction['category']}: {transaction['amount']:.2f}元 - {transaction['description']}\033[0m")

# 运行系统
if __name__ == "__main__":
    system = PersonalFinanceSystem()
    system.run()
```