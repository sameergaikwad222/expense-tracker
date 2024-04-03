<template>
  <div id="balanceShow" class="my-4 pa-5">
    <div class="card px-5 py-3 text-center text-success">
      <h2>INCOME</h2>
      <h2>${{ income }}</h2>
    </div>
    <div class="card px-5 py-3 text-center text-danger">
      <h2>EXPENSE</h2>
      <h2>${{ expense }}</h2>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  income: {
    type: Number,
    required: false,
    default: 0,
  },
  expense: {
    type: Number,
    required: false,
    default: 0,
  },
});
</script>

<style scoped>
#balanceShow {
  display: flex;
  justify-content: space-around;
}
</style>
