<script setup>
import { formatNumber } from "@/helpers/utils";

defineProps({
  subtotal: {
    type: Number,
    required: true,
  },
  itemCount: {
    type: Number,
    required: true,
  },
  barcode: {
    type: String,
    required: true,
  },
  isProcessing: {
    type: Boolean,
    default: false,
  },
  formProcessing: {
    type: Boolean,
    default: false,
  },
});

defineEmits(["update:barcode", "add-item", "process-payment"]);
</script>

<template>
  <div class="column">
    <div class="row justify-end items-center q-gutter-sm">
      <span class="text-weight-bold text-grey-8">GRAND TOTAL: Rp.</span>
      <span class="text-h4 text-weight-bold text-primary">
        {{ formatNumber(subtotal) }}
      </span>
    </div>
    <div class="text-caption text-grey-6 q-mt-xs text-right">
      {{ itemCount }} item(s)
    </div>

    <div class="q-py-sm">
      <q-input
        :model-value="barcode"
        @update:model-value="(val) => $emit('update:barcode', val)"
        placeholder="<Input Barcode>"
        outlined
        square
        class="col bg-white"
        @keyup.enter="$emit('add-item')"
        :loading="isProcessing"
        clearable
        autofocus
      >
        <template v-slot:prepend>
          <q-icon name="qr_code_scanner" />
        </template>
      </q-input>
    </div>

    <div class="q-py-sm">
      <q-btn
        class="full-width"
        label="Bayar"
        color="primary"
        icon="payment"
        @click="$emit('process-payment')"
        :disable="itemCount === 0"
        :loading="formProcessing"
      />
    </div>
  </div>
</template>
