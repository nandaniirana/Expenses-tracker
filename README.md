# Expenses-tracker
import json
from datetime import datetime


# Global list to store expense data
expenses = []


# Function to add an expense
def add_expense():
    """Prompts the user for an expense and adds it to the list."""
    try:
        # Get user inputs
        amount = float(input("Enter the amount spent: "))
        category = input("Enter the category (e.g., groceries, transport, entertainment): ").strip()
        
        # Ensure category is not empty
        if not category:
            raise ValueError("Category cannot be empty.")

        date_str = input("Enter the date (YYYY-MM-DD): ")

        # Validate the date format
        date = datetime.strptime(date_str, "%Y-%m-%d").date()

        # Store the expense in the expenses list
        expense = {
            "amount": amount,
            "category": category,
            "date": date
        }
        expenses.append(expense)
        print(f"Expense added: {expense}")

    except ValueError as e:
        print(f"Error: {e}. Please enter valid data.")


# Function to view the expenses by category
def view_by_category():
    """Displays expenses for a given category and the total amount spent."""
    category = input("Enter category to view: ").lower()

    total_spent = 0
    print(f"\nExpenses in category '{category}':")
    found = False
    for expense in expenses:
        if expense['category'].lower() == category:
            print(f"Date: {expense['date']} | Amount: ${expense['amount']}")
            total_spent += expense['amount']
            found = True
    
    if not found:
        print(f"No expenses found in the '{category}' category.")
    else:
        print(f"\nTotal spent in {category}: ${total_spent}")


# Function to view the summary of expenses by month
def monthly_summary():
    """Shows the total spending for a specific month and year."""
    try:
        month = int(input("Enter month to view summary (1-12): "))
        year = int(input("Enter year: "))

        if not (1 <= month <= 12):
            raise ValueError("Month must be between 1 and 12.")

        total_spent = 0
        print(f"\nExpenses for {year}-{month:02d}:")
        found = False
        for expense in expenses:
            if expense['date'].year == year and expense['date'].month == month:
                print(f"Date: {expense['date']} | Amount: ${expense['amount']} | Category: {expense['category']}")
                total_spent += expense['amount']
                found = True

        if not found:
            print(f"No expenses found for {year}-{month:02d}.")
        else:
            print(f"\nTotal spent in {year}-{month:02d}: ${total_spent}")

    except ValueError as e:
        print(f"Error: {e}. Please enter valid month and year.")


# Function to delete an expense
def delete_expense():
    """Deletes an expense based on user input."""
    try:
        print("\nExisting Expenses:")
        for idx, expense in enumerate(expenses):
            print(f"{idx + 1}. Date: {expense['date']} | Category: {expense['category']} | Amount: ${expense['amount']}")
        
        index = int(input("Enter the number of the expense you want to delete: "))
        
        # Check if the index is valid
        if 1 <= index <= len(expenses):
            deleted_expense = expenses.pop(index - 1)
            print(f"Expense deleted: {deleted_expense}")
        else:
            print("Invalid index. No expense was deleted.")

    except ValueError:
        print("Invalid input. Please enter a valid number.")


# Function to update an expense
def update_expense():
    """Updates the details of an existing expense."""
    try:
        print("\nExisting Expenses:")
        for idx, expense in enumerate(expenses):
            print(f"{idx + 1}. Date: {expense['date']} | Category: {expense['category']} | Amount: ${expense['amount']}")

        index = int(input("Enter the number of the expense you want to update: "))

        # Check if the index is valid
        if 1 <= index <= len(expenses):
            expense = expenses[index - 1]

            print("\nUpdating expense:")
            new_amount = input(f"Enter the new amount (current: ${expense['amount']}): ")
            new_category = input(f"Enter the new category (current: {expense['category']}): ").strip()
            new_date = input(f"Enter the new date (current: {expense['date']}): ")

            # Validate new inputs and apply changes
            if new_amount:
                expense['amount'] = float(new_amount)
            if new_category:
                expense['category'] = new_category
            if new_date:
                expense['date'] = datetime.strptime(new_date, "%Y-%m-%d").date()

            print(f"Expense updated: {expense}")
        else:
            print("Invalid index. No expense was updated.")

    except ValueError as e:
        print(f"Error: {e}. Please enter valid data.")


# Function to save the expenses to a file
def save_expenses():
    """Saves all expenses to a JSON file."""
    try:
        with open("expenses.json", "w") as file:
            json.dump(expenses, file)
        print("\nExpenses saved successfully.")
    except Exception as e:
        print(f"Error saving expenses: {e}")


# Function to load expenses from a file
def load_expenses():
    """Loads expenses from a JSON file."""
    try:
        global expenses
        with open("expenses.json", "r") as file:
            expenses = json.load(file)
        print("\nExpenses loaded successfully.")
    except Exception as e:
        print(f"Error loading expenses: {e}")


# Function to display the main menu
def display_menu():
    """Displays the main menu."""
    print("\nExpense Tracker Menu:")
    print("1. Add an expense")
    print("2. View expenses by category")
    print("3. View monthly summary")
    print("4. Update an expense")
    print("5. Delete an expense")
    print("6. Save expenses")
    print("7. Load expenses")
    print("8. Exit")


# Main function to run the Expense Tracker
def main():
    """Main loop that runs the Expense Tracker application."""
    print("Welcome to the Expense Tracker!")

    while True:
        display_menu()

        choice = input("Choose an option (1-8): ")

        if choice == "1":
            add_expense()
        elif choice == "2":
            view_by_category()
        elif choice == "3":
            monthly_summary()
        elif choice == "4":
            update_expense()
        elif choice == "5":
            delete_expense()
        elif choice == "6":
            save_expenses()
        elif choice == "7":
            load_expenses()
        elif choice == "8":
            print("Exiting Expense Tracker. Goodbye!")
            break
        else:
            print("Invalid choice, please try again.")


if __name__ == "__main__":
    main()
