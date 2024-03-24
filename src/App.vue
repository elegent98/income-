<template>
  <div>
    <Header />
    <div class="container">
      <Balance :total="+total" />
      <IncomeExpence :income="+income" :expenses="+expenses" />
      <TransActionList
        :transactions="transactions"
        @transactionDeleted="handlerTransactionDeleted"
      />
      <AddTransaction @transactionSubmitted="handlerTransactionSubmitted" />
    </div>
  </div>
</template>
<script setup>
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpence from "./components/IncomeExprens.vue";
import TransActionList from "./components/TransActionList.vue";
import AddTransaction from "./components/AddTransaction.vue";

import { useToast } from "vue-toastification";

import { ref, computed, onMounted } from "vue";

const toast = useToast();
onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem("transactions"));
  if (savedTransactions) {
    transactions.value = savedTransactions;
  }
});
const transactions = ref([
  { id: 1, text: "Flower", amount: -19.99 },
  { id: 2, text: "Salary", amount: 299.97 },
  { id: 3, text: "Book", amount: -10 },
  { id: 4, text: "Camera", amount: 150 },
]);
const total = computed(() => {
  return transactions.value.reduce((acc, transactions) => {
    return acc + transactions.amount;
  }, 0);
});

// Income
const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transactions) => {
      return acc + transactions.amount;
    }, 0)
    .toFixed(2);
});
// Get expenses
const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transactions) => {
      return acc + transactions.amount;
    }, 0)
    .toFixed(2);
});
const handlerTransactionSubmitted = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount,
  });
  savedTransactionsLocalStorage();
  toast.success("Tranzaksiya muvaffaqiyatli qo'shildi");
};
const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000);
};
const handlerTransactionDeleted = (id) => {
  transactions.value = transactions.value.filter(
    (transaction) => transaction.id !== id
  );
  savedTransactionsLocalStorage();
  toast.success("Tranzaksiya muvaffaqiyatli o'chirildi");
};
const savedTransactionsLocalStorage = () => {
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
};
</script>
<style></style>
