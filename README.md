import pandas as pd
import matplotlib.pyplot as plt
from datetime import datetime
import os

# File Path
FILE_NAME = 'expenses.csv'

# Create CSV if not exists
if not os.path.exists(FILE_NAME):
    df = pd.DataFrame(columns=['Date', 'Category', 'Amount', 'Description'])
    df.to_csv(FILE_NAME, index=False)


def add_expense():
    date = datetime.now().strftime('%Y-%m-%d')
    category = input('Enter Category (Food/Travel/Bills/etc): ')

    try:
        amount = float(input('Enter Amount: '))
    except ValueError:
        print('Invalid amount!')
        return

    description = input('Enter Description: ')

    new_data = {
        'Date': date,
        'Category': category,
        'Amount': amount,
        'Description': description
    }

    df = pd.read_csv(FILE_NAME)
    df = pd.concat([df, pd.DataFrame([new_data])], ignore_index=True)
    df.to_csv(FILE_NAME, index=False)

    print('\nExpense Added Successfully!')


def view_expenses():
    df = pd.read_csv(FILE_NAME)

    if df.empty:
        print('\nNo expenses found!')
        return

    print('\nAll Expenses:\n')
    print(df)


def monthly_summary():
    df = pd.read_csv(FILE_NAME)

    if df.empty:
        print('\nNo data available!')
        return

    summary = df.groupby('Category')['Amount'].sum()

    print('\nMonthly Expense Summary:\n')
    print(summary)


def generate_chart():
    df = pd.read_csv(FILE_NAME)

    if df.empty:
        print('\nNo data available for chart!')
        return

    summary = df.groupby('Category')['Amount'].sum()

    plt.figure(figsize=(8, 5))
    summary.plot(kind='bar')

    plt.title('Expense by Category')
    plt.xlabel('Category')
    plt.ylabel('Amount')

    plt.tight_layout()
    plt.savefig('expense_chart.png')

    print('\nChart Saved Successfully as expense_chart.png')


def highest_spending_category():
    df = pd.read_csv(FILE_NAME)

    if df.empty:
        print('\nNo data available!')
        return

    summary = df.groupby('Category')['Amount'].sum()
    highest = summary.idxmax()

    print(f'\nHighest Spending Category: {highest}')
    print(f'Total Spent: ₹{summary.max()}')


def main_menu():
    while True:
        print('\n===== SMART EXPENSE TRACKER =====')
        print('1. Add Expense')
        print('2. View Expenses')
        print('3. Monthly Summary')
        print('4. Generate Expense Chart')
        print('5. Highest Spending Category')
        print('6. Exit')

        choice = input('Enter Your Choice: ')

        if choice == '1':
            add_expense()

        elif choice == '2':
            view_expenses()

        elif choice == '3':
            monthly_summary()

        elif choice == '4':
            generate_chart()

        elif choice == '5':
            highest_spending_category()

        elif choice == '6':
            print('\nThank You for Using Smart Expense Tracker!')
            break

        else:
            print('\nInvalid Choice! Try Again.')


if __name__ == '__main__':
    main_menu()
