<template>
  <UModal v-model="isOpen">
    <UCard>
      <template #header>
        <div class="flex justify-between items-center w-full">
          <span>Add Transaction</span>
          <UButton color="gray" variant="ghost" icon="i-heroicons-x-mark-20-solid" class="-my-1"
                   @click="isOpen = false"/>
        </div>
      </template>
      <UForm :state="state" :schema="schema" ref="form" @submit="save">
        <UFormGroup label="Transaction type" :required="true" name="type" class="mb-4">
          <USelect placeholder="Select Transaction type" :options="types" v-model="state.type"/>
        </UFormGroup>
        <UFormGroup label="Amount" :required="true" name="amount" class="mb-4">
          <UInput type="number" placeholder="Amount" v-model.number="state.amount"/>
        </UFormGroup>
        <UFormGroup label="Transaction date" :required="true" name="created_at" class="mb-4">
          <UInput type="date" icon="i-heroicons-calendar-days-20-solid" v-model="state.created_at"/>
        </UFormGroup>
        <UFormGroup label="Description" hint="Optionnal" name="description" class="mb-4">
          <UInput placeholder="Description" v-model="state.description"/>
        </UFormGroup>
        <UFormGroup :required="true" label="Category" name="category" class="mb-4"
                    v-if="state.type === 'Income' || state.type === 'Expense'">
          <USelect placeholder="Select Category" :options="availableCategories" v-model="state.category"/>
        </UFormGroup>
        <UButton type="submit" color="black" variant="solid" :loading="isLoading">Save</UButton>
      </UForm>
    </UCard>
  </UModal>
</template>

<script setup>
import {expenseCategories, incomeCategories, types} from '~/constants';
import {z} from 'zod';

const props = defineProps({
  modelValue: Boolean
})

const emit = defineEmits(['update:modelValue', 'saved']);

const defaultSchema = z.object({
  created_at: z.string(),
  description: z.string(),
  amount: z.number().positive('Amount most be more than 0'),
})
const incomeSchema = z.object({
  type: z.literal('Income'),
  category: z.enum(incomeCategories),
})
const expenseSchema = z.object({
  type: z.literal('Expense'),
  category: z.enum(expenseCategories),
})
const investmentSchema = z.object({
  type: z.literal('Investment'),
})
const savingSchema = z.object({
  type: z.literal('Saving'),
})
const schema = z.intersection(
    z.discriminatedUnion('type', [incomeSchema, expenseSchema, investmentSchema, savingSchema]),
    defaultSchema
)

const supabase = useSupabaseClient();
const form = ref();
const isLoading = ref(false);
const toast = useToast();

const save = async () => {
  if (form.value.errors.length) return
  isLoading.value = true;
  try {
    const {error} = await supabase
        .from('transactions')
        .upsert({...state.value});
    if (!error) {
      toast.add({
        'title': 'Transaction Saved',
        'icon': 'i-heroicons-check-circle'
      })
      isOpen.value = false;
      emit('saved');
      return;
    }
    throw error;
  } catch (e) {
    toast.add({
      'title': 'Transaction not saved',
      description: e.message,
      icon: 'i-heroicons-exclamation-circle',
      color: 'red'
    })
  } finally {
    isLoading.value = true;
  }
}

const initialState = {
  type: undefined,
  amount: 0,
  created_at: undefined,
  description: undefined,
  category: undefined
};

const state = ref(initialState);

const resetForm = () => {
  Object.assign(state.value, initialState);
  form.value.clear();
}

const isOpen = computed({
  get: () => props.modelValue,
  set: (value) => {
    if (!value) resetForm();
    emit('update:modelValue', value)
  }
})

const availableCategories = computed(() => {
  return state.value.type === 'Income' ? incomeCategories : expenseCategories;
});
</script>