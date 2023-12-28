<template>
    <h3>Add new transaction</h3>
      <form id="form" @submit.prevent="onSubmit">
        <div class="form-control">
          <label for="text">Text</label>
          <input type="text" id="text" v-model="text" placeholder="Enter text..." />
        </div>
        <div class="form-control">
          <label for="amount"
            >Amount <br />
            (negative - expense, positive - income)</label
          >
          <input type="number" id="amount" step="0.01" v-model="amount" placeholder="Enter amount..." />
        </div>
        <button class="btn">Add transaction</button>
      </form>
</template>

<script setup>
import { ref } from 'vue'
import { useToast } from 'vue-toastification'

const toast = useToast()

const emit = defineEmits(['transactionSubmitted'])

const text = ref('')
const amount = ref(0)
const addTransaction = () => {

const transactionData = {
    text: text.value,
    amount: parseFloat(amount.value)
}

emit('transactionSubmitted', transactionData)

text.value = ''
amount.value = ''
}
const onSubmit = () => {
    if (!text.value || !amount.value) {
        toast.error('Both fields are required')
    }else{
        addTransaction()
    }
    
    
}
</script>