<template>
  <div class="container my-5 text-center w-25">
    <!-- Button trigger modal -->
    <button
      type="button"
      class="btn btn-primary btn-lg"
      data-bs-toggle="modal"
      data-bs-target="#exampleModal"
    >
      Add Transactions
    </button>

    <!-- Modal -->
    <div
      class="modal fade"
      id="exampleModal"
      tabindex="-1"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h1 class="modal-title fs-5" id="exampleModalLabel">Transcation</h1>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body">
            <form id="form" @submit.prevent="onSubmit" autocomplete="off">
              <!-- txn Label -->
              <div class="mb-3 w-50 mx-auto">
                <label for="txnDetails" class="form-label"
                  >Transaction Label</label
                >
                <input
                  type="text"
                  class="form-control"
                  id="txnDetails"
                  v-model="txn.txnDetails"
                  required
                />
              </div>
              <hr />
              <!-- txn From -->
              <div class="mb-3 w-50 mx-auto">
                <label for="txnFrom" class="form-label">Transaction From</label>
                <input
                  type="text"
                  class="form-control"
                  id="txnFrom"
                  v-model="txn.txnFrom"
                  required
                />
              </div>
              <hr />
              <!-- txn To -->
              <div class="mb-3 w-50 mx-auto">
                <label for="txnTo" class="form-label">Transaction To</label>
                <input
                  type="text"
                  class="form-control"
                  id="txnTo"
                  v-model="txn.txnTo"
                  required
                />
              </div>
              <hr />
              <!-- txn Amount -->
              <div class="mb-3 w-50 mx-auto">
                <label for="txnAmount" class="form-label"
                  >Transaction Amount</label
                >
                <input
                  type="text"
                  class="form-control"
                  id="txnAmount"
                  aria-describedby="TransactionText"
                  v-model="txn.txnAmount"
                  required
                />
              </div>
              <hr />
              <!-- txn Type -->
              <div class="my-4 w-50 mx-auto d-flex justify-content-around">
                <div class="form-check">
                  <input
                    class="form-check-input"
                    v-model="txn.txnType"
                    value="debit"
                    type="radio"
                    name="flexRadioDefault"
                    id="debit1"
                    required
                  />
                  <label class="form-check-label text-danger" for="debit1">
                    Debit
                  </label>
                </div>
                <div class="form-check">
                  <input
                    class="form-check-input"
                    type="radio"
                    v-model="txn.txnType"
                    value="credit"
                    name="flexRadioDefault"
                    id="credit2"
                    required
                  />
                  <label class="form-check-label text-success" for="credit2">
                    Credit
                  </label>
                </div>
              </div>

              <div class="my-4 mx-auto d-flex justify-content-around">
                <label for="txnDatetime">Transcation (date and time):</label>
                <input
                  type="datetime-local"
                  id="txnDatetime"
                  name="txnDatetime"
                  v-model="txn.txnDateStamp"
                />
              </div>
              <!-- Submit the Transaction -->
              <button
                type="submit"
                class="btn btn-primary"
                @submit="onSubmit"
                data-bs-dismiss="modal"
              >
                Submit
              </button>
            </form>
          </div>
          <div class="modal-footer">
            <button
              type="button"
              class="btn btn-secondary"
              data-bs-dismiss="modal"
            >
              Close
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import generateUUID from "../util/helper";

const emit = defineEmits(["onSubmitted"]);
const txn = ref({
  txnID: "",
  txnDetails: "",
  txnFrom: "",
  txnTo: "",
  txnAmount: "",
  txnDateStamp: "",
  txnType: "",
});

function onSubmit() {
  txn.value.txnID = generateUUID();
  let transaction = txn.value;
  txn.value = {
    txnID: "",
    txnDetails: "",
    txnFrom: "",
    txnTo: "",
    txnAmount: "",
    txnDateStamp: "",
    txnType: "",
  };
  emit("onSubmitted", transaction);
}
</script>
