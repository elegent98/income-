<template>
    <h3>Yangi tranzaksiya qo'shing</h3>
    <form  id="form" @submit.prevent="onSubmit">
        <div class="form-control">
            <label for="text">Text</label>
            <input type="text" v-model="text" id="text" placeholder="Textni kiriting...">
        </div>
        <div class="form-control">
            <label for="amount">
                Miqdor <br> (salbiy - xarajat, ijobiy - daromad ) 
            </label>
            <input type="text" v-model="amount"  id="amount" placeholder="Miqdorni kiriting...">
        </div>
        <button  class="btn">Tranzaksiya qo'shing</button>
    </form>
</template>

<script setup>

import { ref } from "vue";
import { useToast } from "vue-toastification";

const text = ref("");

const amount = ref("");

const emit = defineEmits(['transactionSubmitted'])

const toast = useToast();

const onSubmit = () => {
    if (!text.value || !amount.value) {
        toast.error("Please fill all fields");
        return;
    }
    const transactionData = {
        text: text.value,
        amount: parseFloat(amount.value)
    }

    emit('transactionSubmitted', transactionData)
    text.value = ''
    amount.value = ''
}

</script>
  