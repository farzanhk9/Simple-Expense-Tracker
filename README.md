import csv
import os
from datetime import datetime

FILE_NAME = "expenses.csv"

def initialize_file():
    if not os.path.exists(FILE_NAME):
        with open(FILE_NAME, mode="w", newline="") as file:
            writer = csv.writer(file)
            writer.writerow(["Date", "Category", "Amount", "Description"])

def add_expense():
    date = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    category = input("Enter category: ")
    
    try:
        amount = float(input("Enter amount: "))
    except ValueError:
        print("Invalid amount.")
        return

    description = input("Enter description: ")

    with open(FILE_NAME, mode="a", newline="") as file:
        writer = csv.writer(file)
        writer.writerow([date, category, amount, description])

    print("Expense added successfully.")

def view_expenses():
    if not os.path.exists(FILE_NAME):
        print("No expenses recorded yet.")
        return

    with open(FILE_NAME, mode="r") as file:
        reader = csv.reader(file)
        for row in reader:
            print(" | ".join(row))

def total_expenses():
    total = 0
    if not os.path.exists(FILE_NAME):
        print("No expenses recorded yet.")
        return

    with open(FILE_NAME, mode="r") as file:
        reader = csv.DictReader(file)
        for row in reader:
            total += float(row["Amount"])

    print(f"Total Expenses: ${total:.2f}")

def main():
    initialize_file()

    while True:
        print("\n=== Expense Tracker ===")
        print("1. Add Expense")
        print("2. View Expenses")
        print("3. Show Total")
        print("4. Exit")

        choice = input("Choose an option: ")

        if choice == "1":
            add_expense()
        elif choice == "2":
            view_expenses()
        elif choice == "3":
            total_expenses()
        elif choice == "4":
            print("Goodbye!")
            break
        else:
            print("Invalid choice.")

if __name__ == "__main__":
    main()
