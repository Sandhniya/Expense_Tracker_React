# Expense Tracker (ReactJS)
## Date:24.05.25

## AIM
To develop a simple Expense Tracker application using React that allows users to manage their personal finances by adding, viewing, and deleting income and expense transactions, while dynamically calculating the current balance, total income, and total expenses.

## ALGORITHM
### STEP 1: Initialize the Project
Create a new React app using:

npx create-react-app expense-tracker
or

npm create vite@latest expense-tracker --template react

Open the project in a code editor like VS Code.

### Step 2: Setup State
Define a state variable to store transactions:

Example: const [transactions, setTransactions] = useState([])

Define state variables for the form inputs:

const [text, setText] = useState("")

const [amount, setAmount] = useState("")

### Step 3: Add a New Transaction
Create a form with two inputs:

Text input for description

Number input for amount

On form submit:

Validate input

Create a new transaction object

Add the object to the transactions array using setTransactions.

### Step 4: Display Transaction List

Use map() to render each transaction in a list.

Conditionally style each item based on amount > 0 (income) or amount < 0 (expense).

Add a delete button next to each transaction.

### Step 5: Calculate and Display Summary

Use reduce() to calculate:

Total Balance: sum of all amounts

Total Income: sum of all positive amounts

Total Expenses: sum of all negative amounts

Display these values at the top of the UI.

### Step 6: Delete a Transaction

When delete is clicked:

Use filter() to remove the transaction from the array by id.

Update the state using setTransactions.

### Step 7: Style the Application

Use basic CSS to style:

Balance summary

Income/expense totals

Form inputs

Transaction list (with color coding)

## PROGRAM
APP.JS
```
import { useState } from 'react';
import './App.css';

function App() {
  const [transactions, setTransactions] = useState([]);
  const [description, setDescription] = useState('');
  const [amount, setAmount] = useState('');
  const amounts = transactions.map(transaction => transaction.amount);
  const balance = amounts.reduce((acc, item) => (acc += item), 0).toFixed(2);
  const income = amounts.filter(item => item > 0).reduce((acc, item) => (acc += item), 0).toFixed(2);
  const expense = (amounts.filter(item => item < 0).reduce((acc, item) => (acc += item), 0) * -1).toFixed(2);
  const handleSubmit = (e) => {
    e.preventDefault();
    if (!description.trim() || !amount) return;

    const newTransaction = {
      id: Date.now(),
      description,
      amount: +amount
    };

    setTransactions([...transactions, newTransaction]);
    setDescription('');
    setAmount('');
  };

  const deleteTransaction = (id) => {
    setTransactions(transactions.filter(transaction => transaction.id !== id));
  };

  return (
    <div className="container">
      <h1>ExpensePro</h1>
      
      <div className="balance-container">
        <h2>Your Balance</h2>
        <h3>${balance}</h3>
        
        <div className="income-expense-container">
          <div className="income">
            <h4>Income</h4>
            <p className="money plus">+${income}</p>
          </div>
          <div className="expense">
            <h4>Expense</h4>
            <p className="money minus">-${expense}</p>
          </div>
        </div>
      </div>
      
      <div className="transaction-list">
        <h3>Transactions</h3>
        <ul className="list">
          {transactions.map(transaction => (
            <li key={transaction.id} className={transaction.amount < 0 ? 'minus' : 'plus'}>
              {transaction.description} 
              <span>
                {transaction.amount < 0 ? '-' : '+'}${Math.abs(transaction.amount).toFixed(2)}
              </span>
              <button 
                onClick={() => deleteTransaction(transaction.id)}
                className="delete-btn"
              >
                x
              </button>
            </li>
          ))}
        </ul>
      </div>
      
      <div className="add-transaction">
        <h3>Add New Transaction</h3>
        <form onSubmit={handleSubmit}>
          <div className="form-control">
            <label htmlFor="description">Description</label>
            <input
              type="text"
              id="description"
              value={description}
              onChange={(e) => setDescription(e.target.value)}
              placeholder="Enter description..."
              required
            />
          </div>
          <div className="form-control">
            <label htmlFor="amount">Amount</label>
            <p>(negative - expense, positive - income)</p>
            <input
              type="number"
              id="amount"
              value={amount}
              onChange={(e) => setAmount(e.target.value)}
              placeholder="Enter amount..."
              required
            />
          </div>
          <button className="btn">Add Transaction</button>
        </form>
      </div>
    </div>
  );
}

export default App;
```
APP.CSS
```
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: 'Arial', sans-serif;
  line-height: 1.6;
  background-color: #f5f5f5;
  color: #333;
}

.container {
  max-width: 500px;
  margin: 30px auto;
  padding: 20px;
  background-color: #fff;
  border-radius: 5px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

h1 {
  text-align: center;
  margin-bottom: 20px;
  color: #2c3e50;
}

h2, h3, h4 {
  margin: 10px 0;
}

.balance-container {
  text-align: center;
  margin-bottom: 20px;
}

.balance-container h3 {
  font-size: 2rem;
  color: #2c3e50;
}

.income-expense-container {
  display: flex;
  justify-content: space-around;
  background-color: #fff;
  padding: 15px;
  margin: 15px 0;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
}

.income, .expense {
  text-align: center;
}

.income h4 {
  color: #2ecc71;
}

.expense h4 {
  color: #e74c3c;
}

.money {
  font-size: 1.2rem;
  font-weight: bold;
  margin-top: 5px;
}

.plus {
  color: #2ecc71;
}

.minus {
  color: #e74c3c;
}

.list {
  list-style-type: none;
  margin: 20px 0;
}

.list li {
  background-color: #fff;
  padding: 10px;
  margin: 10px 0;
  display: flex;
  justify-content: space-between;
  position: relative;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
}

.list li.plus {
  border-right: 5px solid #2ecc71;
}

.list li.minus {
  border-right: 5px solid #e74c3c;
}

.delete-btn {
  background-color: #e74c3c;
  color: white;
  border: none;
  padding: 2px 5px;
  cursor: pointer;
  position: absolute;
  left: -20px;
  opacity: 0;
  transition: opacity 0.3s ease;
}

.list li:hover .delete-btn {
  opacity: 1;
}

.form-control {
  margin: 10px 0;
}

.form-control label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
}

.form-control input {
  width: 100%;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 16px;
}

.btn {
  width: 100%;
  padding: 10px;
  background-color: #2c3e50;
  color: white;
  border: none;
  border-radius: 4px;
  font-size: 16px;
  cursor: pointer;
  margin-top: 10px;
}

.btn:hover {
  background-color: #1a252f;
}

@media (max-width: 500px) {
  .container {
    margin: 15px;
    padding: 15px;
  }
  
  .income-expense-container {
    flex-direction: column;
  }
  
  .income, .expense {
    margin: 5px 0;
  }
}
```


## OUTPUT
![image](https://github.com/user-attachments/assets/5738c7d8-53c6-483b-954e-39983ed00061)


## RESULT
A fully functional React-based Expense Tracker application was successfully developed. 
