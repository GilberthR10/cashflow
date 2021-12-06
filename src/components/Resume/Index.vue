<template>
  <main>
    <p>{{ labelVisual }}</p>
    <h1>{{ amountCurrency }}</h1>
    <div class="graphic">
      <slot name="graphic"></slot>
    </div>
    <div class="action">
      <slot name="action"></slot>
    </div>
  </main>
</template>

<script setup>
import { defineProps, computed, toRefs } from "vue";

const currencyFormatter = new Intl.NumberFormat("es-CO", {
  style: "currency",
  currency: "COP",
});

const props = defineProps({
  totalLabel: {
    type: String,
  },
  label: {
    type: String,
    default: null,
  },
  totalAmount: {
    type: Number,
  },
  amount: {
    type: Number,
    default: null,
  },
});

const { label, totalLabel, totalAmount, amount } = toRefs(props);

const labelVisual = computed(() =>
  label.value !== null ? label.value : totalLabel.value
);

const amountVisual = computed(() =>
  amount.value !== null ? amount.value : totalAmount.value
);

const amountCurrency = computed(() =>
  currencyFormatter.format(amountVisual.value)
);
</script>

<style scoped>
main {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  width: 100%;
}

h1,
p {
  margin: 0;
  text-align: center;
}

h1 {
  margin-top: 14px;
  color: var(--brand-green);
}

.graphic {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  padding: 48px 24px;
  box-sizing: border-box;
}
</style>
