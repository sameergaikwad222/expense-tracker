<template>
  <div class="card text-bg-primary" style="width: 18rem">
    <div class="card-body text-center">
      <h4>YOUR BALANCE</h4>
      <h1>${{ balance }}</h1>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  balance: {
    type: Number,
    required: true,
  },
});
</script>
