import json

class ExpenseTracker:
    def _init_(self, filename="expenses.json"):
        self.filename = filename
        self.expenses = []
        self.budgets = {}
        self.load_expenses()

    def add_expense(self, amount, category, description):
        expense = {"amount": amount, "category": category, "description": description}
        self.expenses.append(expense)
        self.save_expenses()
        print("Expense added successfully")

    def set_budget(self, category, amount):
        self.budgets[category] = amount
        self.save_expenses()
        print(f"Budget set for {category}: ${amount}")

    def view_expenses(self):
        if not self.expenses:
            print("No expenses recorded.")
            return
        for idx, exp in enumerate(self.expenses, 1):
            print(f"{idx}. {exp['category']} - ${exp['amonut']} ({exp['description']})")

    def view_budget_status(self):
        total_spent = {}
        for exp in self.expenses:
            total_spent[exp["category"]] = total_spent.get(exp["category"], 0) + exp["amount"]

        for category, budget in self.budgets.item():
            spent = total_spent.get(category, 0)
            print(f"Category: {category} | Spent: ${spent} | Budget: ${budget} | Remaining: ${budget - spent}")

    def save_expenses(self):
        data = {"expenses": self.expenses, "budgets": self.budgets}
        with open(self.filename, "w") as f:
            json.dump(data, f)

    def load_expenses(self):
        try:
            with open(self.filename, "r") as f:
                data = json.load(f)
                self.expenses = data.get("expenses", [])
                self.budgets = data.get("budgets", {})
        except FileNotFoundError:
            self.expenses = []
            self.budgets = {}

    def menu(self):
        while True:
            print("\nExpense Tracker Menu:")
            print("1. Add Expense")
            print("2. Set Budget")
            print("3. View Expenses")
            print("4. View Budget Status")
            print("5. Exit")
            choice = input("choose an option: ")

            if choice == "1":
                amount = float(input("Enter amount: "))
                category = input("Enter category: ")
                description = input("Enter description: ")
                self.add_expense(amount, category, description)
            elif choice == "2":
                category = input("Enter category: ")
                amount = float(input("Enter budget amount: "))
                self.set_budget(category, amount)
            elif choice == "3":
                self.view_expenses()
            elif choice == "4":
                self.view_budget_status()
            elif choice == "5":
                print("Exiting program. Goodbye!")
                break
            else:
                print("Invalid choice. Please try again.")

if __name__== "__main__":
    tracker = ExpenseTracker()
    tracker.menu()
