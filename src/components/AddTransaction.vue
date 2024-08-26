<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input type="text" v-model="text" id="text" placeholder="Enter text..." />
    </div>
    <div class="form-control">
      <label for="amount">Amount <br />
        (negative - expense, positive - income)</label>
      <input type="number" v-model="number" id="amount" placeholder="Enter amount..." />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>

<script setup>
import { ref } from 'vue';
import { useToast } from 'vue-toastification';

  const text=ref('')
  const number=ref('')

  const emit=defineEmits(['trasactionSubmitted'])

  const toast=useToast()

  const onSubmit=()=>{
    if(!text.value || !number.value){
      toast.error('Both are empty!')
      return
    }
    const transaction={
      text:text.value,
      number:number.value
    }

    emit('trasactionSubmitted',transaction)
    text.value=''
    number.value=''
  }
</script>