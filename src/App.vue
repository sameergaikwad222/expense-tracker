<template>
  <div class="centerApp">
    <HeaderComp class="mb-5 mt-4" />
    <div class="container-fluid">
      <div class="row w-100">
        <div class="col-6">
          <BalanceComp class="w-25 mx-auto mb-5" :balance="balance" />
          <hr />
          <h2 class="mx-auto text-center mt-5">Add New Transaction</h2>
          <AddTranscationsComp @onSubmitted="handleTransaction" />
        </div>
        <div class="col-6">
          <IncomeExpenseComp
            class="w-75 mx-auto my-5"
            :income="income"
            :expense="expense"
          />
          <hr />
          <TransactionListComp
            class="w-75 mx-auto mt-5"
            :txns="txns"
            @onDeletion="handleDeletion"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import BalanceComp from "./components/BalanceComp.vue";
import IncomeExpenseComp from "./components/IncomeExpenseComp.vue";
import HeaderComp from "./components/HeaderComp.vue";
import TransactionListComp from "./components/TransactionListComp.vue";
import AddTranscationsComp from "./components/AddTransactionsComp.vue";

import { ref, computed, onMounted } from "vue";
import { toast } from "vue3-toastify";
import "vue3-toastify/dist/index.css";
// Data
const txns = ref([
  {
    txnID: "73ff0af7-c9fd-4aee-93cb-cb44e9f873f7",
    txnDetails: "Paycheck",
    txnFrom: "Sanghavi Pvt Ltd",
    txnTo: "Self",
    txnAmount: "100.00",
    txnDateStamp: "2024-04-01T20:03:06.490Z",
    txnType: "credit",
  },
  {
    txnID: "5366b9e8-f0e5-47cf-ad9a-b407298103d5",
    txnDetails: "Grocery",
    txnFrom: "Self",
    txnTo: "Retail Mart Pvt Ltd",
    txnAmount: "25.00",
    txnDateStamp: "2024-04-01T21:03:06.490Z",
    txnType: "debit",
  },
  {
    txnID: "c46a8d8c-fed3-455c-9f34-36db98a4f3c7",
    txnDetails: "Rent",
    txnFrom: "Mr. Saheel Khanna",
    txnTo: "Self",
    txnAmount: "125.00",
    txnDateStamp: "2024-04-01T22:03:06.490Z",
    txnType: "credit",
  },
  {
    txnID: "8e9402a8-1f08-4c16-921e-7c00565ea48c",
    txnDetails: "Shopping",
    txnFrom: "Self",
    txnTo: "Toy Market",
    txnAmount: "15.00",
    txnDateStamp: "2024-04-01T22:03:06.490Z",
    txnType: "debit",
  },
]);

// ================================== Hooks ==================================
onMounted(() => {
  const savedTxns = JSON.parse(localStorage.getItem("txns"));

  if (savedTxns && savedTxns.length > 0) {
    txns.value = savedTxns;
  }
});

// ================================== Methods ==================================
const handleTransaction = (txn) => {
  txns.value.push(txn);
  if (txn.txnType == "credit") {
    notify("Credit Transaction Added", "success");
  } else {
    notify("Debit Transaction Added", "warn");
  }
  saveTxnsToLocalStorage();
};

const notify = (text = "default text", classType) => {
  switch (classType) {
    case "danger":
      toast.error(text, {
        autoClose: 1000,
      });
      break;
    case "info":
      toast.info(text, {
        autoClose: 1000,
      });
      break;
    case "warn":
      toast.warn(text, {
        autoClose: 1000,
      });
      break;
    case "success":
      toast.success(text, {
        autoClose: 1000,
      });
      break;
    default:
      toast(text, {
        autoClose: 1000,
      });
      break;
  }
};

const handleDeletion = (txnID) => {
  txns.value = txns.value.filter((t) => t.txnID !== txnID);
  notify("Deleted Transaction Entry", "danger");
  saveTxnsToLocalStorage();
};

const saveTxnsToLocalStorage = () => {
  localStorage.setItem("txns", JSON.stringify(txns.value));
};

// ================================== Computed Propertiese ==================================
const balance = computed(() => {
  return txns.value.reduce((acc, txn) => {
    return txn.txnType == "credit"
      ? acc + parseFloat(txn.txnAmount)
      : acc - parseFloat(txn.txnAmount);
  }, 0);
});

const income = computed(() => {
  return txns.value
    .filter((txn) => {
      return txn.txnType == "credit";
    })
    .reduce((acc, txn) => {
      return acc + parseFloat(txn.txnAmount);
    }, 0);
});

const expense = computed(() => {
  return txns.value
    .filter((txn) => {
      return txn.txnType == "debit";
    })
    .reduce((acc, txn) => {
      return acc + parseFloat(txn.txnAmount);
    }, 0);
});
</script>

<style scoped></style>
