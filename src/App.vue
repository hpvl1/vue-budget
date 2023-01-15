<script setup>
import { computed, reactive } from '@vue/runtime-core';
import BudgetList from './components/BudgetList.vue';
import TotalBalance from './components/TotalBalance.vue';
import Form from './components/Form.vue';


const state = reactive({
  list: [
    {
      id: 1,
      type: 'INCOME',
      value: 100,
      comment: 'Some comment 1',
    },
    {
      id: 2,
      type: 'INCOME',
      value: 200,
      comment: 'Some comment 2',
    },
    {
      id: 3,
      type: 'OUTCOME',
      value: 300,
      comment: 'Some comment 3',
    },
  ],
});

const totalBalance = computed(() => {
  return state.list.reduce((acc, item) => acc + item.value, 0);
});

function onDeleteItem(id) {
  state.list = state.list.filter((item) => item.id !== id);
}

function checkTypeItem(data) {
  data.type === 'OUTCOME' ? (data.value *= -1) : data.value;
}

function setIdItem(data) {
  data.id = state.list[state.list.length - 1].id + 1;
}

function onFormSubmit(data) {
  setIdItem(data);
  checkTypeItem(data);
  state.list.push(data);
}
</script>

<template>
  <div class="mx-auto">
    <v-container>
      <Form @submit-form="onFormSubmit" />
      <TotalBalance :total="totalBalance" />
      <BudgetList :list="state.list" @on-delete-item="onDeleteItem" />
    </v-container>
  </div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
