<template>
     <h3>History</h3>
     <div>
       <ul v-if="transactions.length" id="list" class="list">
         <li v-for="transaction in transactions" 
         :key="transaction.id"
         :class="transaction.amount <0 ? 'minus' : 'plus'">
            <button class="delete-btn" @click="deleteTransaction(transaction.id)">x</button>
           {{  transaction.text }} 
           <span>${{ transaction.amount }}</span>
        </li>  
      </ul>
      <p class="nullHistory" v-else> No transactions made</p>
    </div>
</template>
 
<script setup>
    import { defineProps } from 'vue'

    const emit = defineEmits(['transactionDeleted'])
    const props = defineProps({
        transactions: Array,
        required: true        
    })

    const deleteTransaction = (id) => {
        emit('transactionDeleted', id)
    }
</script>