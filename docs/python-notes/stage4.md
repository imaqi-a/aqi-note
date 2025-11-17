<!-- 学习目标：文件读写、异常处理 -->
```python
# stage4_file_handling.py
import json
import os
from datetime import datetime

class FinanceManager:
    def __init__(self, filename="finance_data.json"):
        self.filename = filename
        self.data = self.load_data()
    
    def load_data(self):
        """从文件加载数据"""
        try:
            if os.path.exists(self.filename):
                with open(self.filename, 'r', encoding='utf-8') as f:
                    return json.load(f)
            else:
                # 创建新的数据结构
                return {
                    "income": [],
                    "expenses": [],
                    "savings_goal": 0,
                    "created_date": datetime.now().strftime("%Y-%m-%d")
                }
        except (json.JSONDecodeError, IOError) as e:
            print(f"加载数据时出错: {e}")
            return {"income": [], "expenses": [], "savings_goal": 0}
    
    def save_data(self):
        """保存数据到文件"""
        try:
            with open(self.filename, 'w', encoding='utf-8') as f:
                json.dump(self.data, f, ensure_ascii=False, indent=2)
            print("数据保存成功！")
        except IOError as e:
            print(f"保存数据时出错: {e}")
    
    def add_transaction(self, transaction_type, amount, category, description):
        """添加交易记录"""
        try:
            amount = float(amount)
            if amount <= 0:
                raise ValueError("金额必须大于0")
            
            transaction = {
                "amount": amount,
                "category": category,
                "description": description,
                "date": datetime.now().strftime("%Y-%m-%d %H:%M:%S")
            }
            
            if transaction_type.lower() == "income":
                self.data["income"].append(transaction)
                print(f"✓ 收入记录添加成功: {amount}元")
            elif transaction_type.lower() == "expense":
                self.data["expenses"].append(transaction)
                print(f"✓ 支出记录添加成功: {amount}元")
            else:
                print("错误：交易类型必须是 'income' 或 'expense'")
            
            self.save_data()
            
        except ValueError as e:
            print(f"输入错误: {e}")
        except Exception as e:
            print(f"添加交易时出错: {e}")
    
    def export_report(self, report_filename="financial_report.txt"):
        """导出财务报告到文本文件"""
        try:
            total_income = sum(item["amount"] for item in self.data["income"])
            total_expenses = sum(item["amount"] for item in self.data["expenses"])
            balance = total_income - total_expenses
            
            with open(report_filename, 'w', encoding='utf-8') as f:
                f.write("=" * 50 + "\n")
                f.write("           个人财务报告\n")
                f.write("=" * 50 + "\n\n")
                f.write(f"报告生成时间: {datetime.now().strftime('%Y-%m-%d %H:%M:%S')}\n")
                f.write(f"总收入: {total_income:.2f} 元\n")
                f.write(f"总支出: {total_expenses:.2f} 元\n")
                f.write(f"当前余额: {balance:.2f} 元\n\n")
                
                f.write("收入明细:\n")
                for income in self.data["income"]:
                    f.write(f"  - {income['date']}: {income['amount']}元 ({income.get('category', '无类别')})\n")
                
                f.write("\n支出明细:\n")
                for expense in self.data["expenses"]:
                    f.write(f"  - {expense['date']}: {expense['amount']}元 ({expense['category']} - {expense['description']})\n")
            
            print(f"报告已导出到 {report_filename}")
            
        except IOError as e:
            print(f"导出报告时出错: {e}")

# 使用示例
def main():
    manager = FinanceManager()
    
    print("=== 文件操作财务系统 ===")
    
    # 添加一些示例交易
    manager.add_transaction("income", 8000, "工资", "月度工资")
    manager.add_transaction("expense", 2500, "住房", "房租")
    manager.add_transaction("expense", 1500, "餐饮", "日常饮食")
    manager.add_transaction("income", 500, "兼职", "周末兼职")
    
    # 导出报告
    manager.export_report()
    
    # 显示当前余额
    total_income = sum(item["amount"] for item in manager.data["income"])
    total_expenses = sum(item["amount"] for item in manager.data["expenses"])
    print(f"\n当前财务状况:")
    print(f"总收入: {total_income:.2f} 元")
    print(f"总支出: {total_expenses:.2f} 元")
    print(f"余额: {total_income - total_expenses:.2f} 元")

if __name__ == "__main__":
    main()
```