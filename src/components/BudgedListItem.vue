<script setup>
import { computed } from '@vue/reactivity';

defineProps({
  list: {
    type: Array,
    default() {
      return [];
    },
  },
});
</script>

<template>
  <div class="card mx-auto" v-for="card in list" :key="card.id">
    <v-card class="mt-n1" max-width="450" width="450" max-height="130">
      <p class="card__comment mt-2">
        {{ card.comment }}
      </p>
      <span class="card__value mx-auto">
        {{ card.value }}
      </span>
      <div class="card__delete mb-2">
        <v-btn
          variant="flat"
          class="mx-auto"
          color="error"
          @click="$emit('on-delete-item', card.id)"
          >Удалить</v-btn
        >
      </div>
    </v-card>
  </div>
</template>

<style scoped>
.card {
  display: flex;
  padding: 15px 25px;
  flex-wrap: wrap;
  column-gap: 20px;
  row-gap: 40px;
}
.card__value {
  font-weight: bold;
  font-size: 20px;
}

.card-bg-gr {
  background: #66bb6a;
}

.card-bg-rd {
  background: #ef5350;
}
</style>
