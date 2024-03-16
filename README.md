class Customer:
    def __init__(self, customer_id, name, address, phone_number):
        self.customer_id = customer_id
        self.name = name
        self.address = address
        self.phone_number = phone_number

class Account:
    def __init__(self, account_number, customer, balance=0):
        self.account_number = account_number
        self.customer = customer
        self.balance = balance

    def deposit(self, amount):
        self.balance += amount

    def withdraw(self, amount):
        if self.balance >= amount:
            self.balance -= amount
        else:
            print("Insufficient balance")

class Transaction:
    def __init__(self, transaction_id, account, amount, transaction_type):
        self.transaction_id = transaction_id
        self.account = account
        self.amount = amount
        self.transaction_type = transaction_type
        self.timestamp = datetime.now()

class BankingSystem:
    def __init__(self):
        self.customers = {}
        self.accounts = {}
        self.transactions = {}
        self.last_customer_id = 0
        self.last_account_number = 0
        self.last_transaction_id = 0

    def create_customer(self, name, address, phone_number):
        self.last_customer_id += 1
        customer_id = self.last_customer_id
        customer = Customer(customer_id, name, address, phone_number)
        self.customers[customer_id] = customer
        return customer

    def open_account(self, customer_id):
        if customer_id in self.customers:
            self.last_account_number += 1
            account_number = self.last_account_number
            account = Account(account_number, self.customers[customer_id])
            self.accounts[account_number] = account
            return account
        else:
            print("Customer not found")

    def make_transaction(self, account_number, amount, transaction_type):
        if account_number in self.accounts:
            account = self.accounts[account_number]
            if transaction_type == 'deposit':
                account.deposit(amount)
            elif transaction_type == 'withdraw':
                account.withdraw(amount)
            else:
                print("Invalid transaction type")
                return
            self.last_transaction_id += 1
            transaction_id = self.last_transaction_id
            transaction = Transaction(transaction_id, account, amount, transaction_type)
            self.transactions[transaction_id] = transaction
        else:
            print("Account not found")

