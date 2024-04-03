<template>
  <div>
    <h2>History Transaction</h2>
    <ul class="list-group mb-4">
      <!-- List Section -->
      <li
        v-for="txn in txns"
        :key="txn.txnID"
        :class="
          txn.txnType == 'credit'
            ? 'list-group-item-success'
            : 'list-group-item-danger'
        "
        class="list-group-item d-flex justify-content-left align-items-center"
      >
        <button
          type="button"
          class="bg-danger btn mx-4"
          @click="deleteTransaction(txn)"
        >
          Delete
        </button>
        <span
          >{{ txn.txnDetails }} | Amt. ${{ parseFloat(txn.txnAmount) }}</span
        >
      </li>
    </ul>
  </div>
</template>

<script setup>
const emit = defineEmits(["onDeletion"]);
const props = defineProps({
  txns: {
    type: Array,
    required: true,
  },
});
const deleteTransaction = (txn) => {
  emit("onDeletion", txn.txnID);
};
</script>
