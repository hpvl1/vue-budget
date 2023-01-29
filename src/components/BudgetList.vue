<script setup>
import BudgedListItem from './BudgedListItem.vue';

defineProps({
  list: {
    type: Array,
    default() {
      return [];
    },
  },
});

const mainHeader = 'Budget List';
const emptyListTitle = 'Empty List!';
</script>

<template>
  <div class="budged-list-wrap">
    <v-card :title="mainHeader" class="mx-auto" max-width="500" text="Simple budged list board">
      <template v-if="list.length !== 0">
        <BudgedListItem :list="list" />
      </template>
      <v-alert v-else type="warning" :title="emptyListTitle"></v-alert>
    </v-card>
  </div>
</template>

<style scoped></style>
