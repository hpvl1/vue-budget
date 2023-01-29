<script setup>
import { computed, toRef } from '@vue/reactivity';
import { watchEffect } from 'vue';

const props = defineProps({
  total: {
    type: Number,
    default: 0,
  },
});

let totalNew = null;

const init = async () => {
  return props.total;
};

watchEffect(async () => {
  totalNew = await init();
});


const totalComp = computed(async () => {
  const getTot = toRef(props, 'total');
  console.log("getTot: ", getTot.value);

  if (getTot.value > 0) return 'total-positive';
  if (getTot.value < 0) return 'total-negative';
  if (getTot.value === 0) return 'total-zero';

  // return {
  //   'total-positive': totalNew > 0,
  //   'total-negative': totalNew < 0,
  //   'total-zero': totalNew === 0,
  // }
});
</script>

<template>
  <div class="total-value-wrap">
      <div class="title" :class="totalComp">Balance: {{ total }}</div>
  </div>
</template>

<style scoped>
.total-value-wrap {
  font-size: 26px;
  text-transform: uppercase;
  padding: 20px;
  text-align: center;
}

.total-positive {
  color: red;
}
</style>
