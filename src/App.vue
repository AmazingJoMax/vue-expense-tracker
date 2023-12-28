<template>
    <Header />
    <div class="">
        <Balance :total="+total"/> 
        <IncomeExpenses :income="+income" :expense="+expense"/>
        <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted"/>
        <AddTransaction @transactionSubmitted="handleTransactionSubmitted"/>
    </div>
</template>


<script setup>
import Balance from './components/Balance.vue'
import Header from './components/Header.vue'
import IncomeExpenses from './components/IncomeExpenses.vue'
import TransactionList from './components/TransactionList.vue'
import AddTransaction from './components/AddTransaction.vue'
import { computed, ref } from 'vue'
import { useToast } from 'vue-toastification'

const toast = useToast()
const transactions = ref([])

const savedTransactions = localStorage.getItem('transactions')
if (savedTransactions) {
    transactions.value = JSON.parse(savedTransactions)
}

 
// Get total
const total = computed(() => {
    return transactions.value.reduce((acc, transaction) => acc += transaction.amount, 0).toFixed(2)
})

// Get income
const income = computed(() => {
    return transactions.value
        .filter((transaction) => transaction.amount > 0)
        .reduce((acc, transaction) => acc += transaction.amount, 0).toFixed(2)
})

// Get expense
const expense = computed(() => {
    return transactions.value
        .filter((transaction) => transaction.amount < 0)
        .reduce((acc, transaction) => acc += transaction.amount, 0).toFixed(2)
})

// Add transaction
const handleTransactionSubmitted = (transactionData) => {
  console.log(transactionData);
    transactions.value.push({
        id: generateId(),
        text: transactionData.text,
        amount: transactionData.amount
    })
    saveTransactionsToLocalStorage() 
    toast.success('Transaction added successfully')
}

// Generate random id
const generateId = () => {
    return Math.floor(Math.random() * 100000000)
}

// Delete transaction
const handleTransactionDeleted = (id) => {
    transactions.value = transactions.value.filter((transaction) => transaction.id !== id)
    toast.success('Transaction deleted successfully')
    saveTransactionsToLocalStorage()
}

// save transactions to local storage
const saveTransactionsToLocalStorage = () => {
    localStorage.setItem('transactions', JSON.stringify(transactions.value))
}
</script>