<template>
  <Header/>
  <div id="container">
    <Balance :total="+total"/>
    <IncomeExpenses :income="+income" :expense="+expense"/>
    <TransactionList :transactions="transactionList" @transactionDeleted="handleTransactionDelete"/>
    <AddTransaction @trasactionSubmitted="handleTransactionSubmitted"/>
  </div>
</template>

<script setup>
  import Header from './components/Header.vue';
  import Balance from './components/Balance.vue';
  import IncomeExpenses from './components/IncomeExpenses.vue';
  import TransactionList from './components/TransactionList.vue';
  import AddTransaction from './components/AddTransaction.vue';

  import { computed, onMounted, ref } from 'vue';
  import { useToast } from 'vue-toastification';

  const transactionList = ref([]);

  const toast=useToast()

  //Initialize data
  onMounted(()=>{
    const savedTransaction=JSON.parse(localStorage.getItem('transactions'))
    if(savedTransaction){
      transactionList.value=savedTransaction
    }
  })

  //Total Amount
  const total=computed(()=>{
    return transactionList.value.reduce((acc,transaction)=>{
      return acc+transaction.amount
    },0).toFixed(2)
  });

  //Income
  const income=computed(()=>{
    return transactionList.value.filter((obj)=>obj.amount>0).reduce((acc,transaction)=>{
      return acc+transaction.amount
    },0).toFixed(2)
  });

  //Expense
  const expense=computed(()=>{
    return transactionList.value.filter((obj)=>obj.amount<0).reduce((acc,transaction)=>{
      return acc+transaction.amount
    },0).toFixed(2)
  });

  //Submit a transaction
  const handleTransactionSubmitted=(objTransaction)=>{
    transactionList.value.push({
      id:generateUniqueId(),
      text:objTransaction.text,
      amount:objTransaction.number
    })
    saveToLocalstorage()
    toast.success('Transaction added!')
  }

  //Generate unique id for a transaction
  const generateUniqueId=()=>{
    return Math.floor(Math.random()*1000000)
  }

  //Delete a transaction
  const handleTransactionDelete=(id)=>{
    transactionList.value=transactionList.value.filter(
      (obj)=>obj.id!=id
    )
    saveToLocalstorage()
    toast.error('Transaction deleted!')
  }

  //Transaction handle on localstorage
  const saveToLocalstorage=()=>{
    localStorage.setItem('transactions',JSON.stringify(transactionList.value))
  }
</script>