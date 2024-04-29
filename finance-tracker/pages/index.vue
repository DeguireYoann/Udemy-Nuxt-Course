<template>
  <section class="flex items-center justify-between mb-10">
    <h1 class="text-4xl font-extrabold">
      Summary
    </h1>
    <div>
      <USelectMenu :options="transactionViewOptions" v-model="selectedView"/>
    </div>
  </section>

  <section class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 sm:gap-16 mb-10">
    <Trend color="green" title="Income" :amount="incomeTotal" :last-amount="3000" :loading="isLoading"/>
    <Trend color="red" title="Expense" :amount="expenseTotal" :last-amount="5000" :loading="isLoading"/>
    <Trend color="green" title="Investments" :amount="4000" :last-amount="3000" :loading="isLoading"/>
    <Trend color="red" title="Saving" :amount="4000" :last-amount="4100" :loading="isLoading"/>
  </section>

  <section class="flex justify-between mb-10">
    <div><h2 class="text-2xl font-extrabold">Transaction</h2>
      <div class="text-gray dark:text-gray-400">
        You have {{ incomeCount ?? 0 }} incomes and
        {{ expenseCount ?? 0 }} expenses this period
      </div>
    </div>
    <div>
      <TransactionModal v-model="isOpen" @saved="refreshTransactions()"/>
      <UButton icon="i-heroicons-plus-circle" color="white" variant="solid" label="Add" @click="isOpen=true"/>
    </div>
  </section>

  <section v-if="!isLoading">
    <div v-for="(transactionsOnDay, date) in transactionsGroupedByDate" :key="date" class="mb-10">
      <DailyTransactionSummary :date="date" :transactions="transactionsOnDay"/>
      <Transaction v-for="transaction in transactionsOnDay" :key="transaction.id" :transaction="transaction"
                   @deleted="refreshTransactions()"/>
    </div>
  </section>
  <section v-if="isLoading">
    <USkeleton class="h-8 w-full mb-2" v-for="i in 4" :key="i"/>
  </section>
</template>

<script setup>
import {transactionViewOptions} from '~/constants'

const supabase = useSupabaseClient();
const selectedView = ref(transactionViewOptions[1]);
const transactions = ref([]);
const isLoading = ref(false);
const isOpen = ref(false);
const fetchTransactions = async () => {
  try {
    const {data} = await useAsyncData('transactions', async () => {
      isLoading.value = true;
      const {data, error} = await supabase
          .from('transactions')
          .select()
          .order('created_at', {ascending: false})

      if (error) return []
      return data
    })
    return data.value
  } finally {
    isLoading.value = false;
  }
}
const refreshTransactions = async () => transactions.value = await fetchTransactions();
await refreshTransactions();

const income = computed(() => transactions.value.filter(t => t.type.toLowerCase() === 'income'));
const incomeCount = computed(() => income.value.length);
const expense = computed(() => transactions.value.filter(t => t.type.toLowerCase() === 'expense'));
const expenseCount = computed(() => expense.value.length);
const incomeTotal = computed(() =>
    income.value.length > 0 ?
        income.value.reduce((sum, transaction) => sum + transaction.amount, 0)
        : 0
);
const expenseTotal = computed(() =>
    expense.value.length > 0 ?
        expense.value.reduce((sum, transaction) => sum + transaction.amount, 0)
        : 0
);
const transactionsGroupedByDate = computed(() => {
  let grouped = {};
  for (const transaction of transactions.value) {
    const date = new Date(transaction.created_at).toDateString().split('T')[0];
    if (!grouped[date]) {
      grouped[date] = []
    }
    grouped[date].push(transaction)
  }
  return grouped;
})
</script>