<template>
  <Layout>
    <template #header>
      <Header></Header>
    </template>
    <template #resume>
      <Resume
        :total-label="'Ahorro total'"
        :label="label"
        :total-amount="totalAmount"
        :amount="amount"
      >
        <template #graphic>
          <Graphic :amounts="amounts" @select="select" />
        </template>
        <template #action>
          <Action @create="create" />
        </template>
      </Resume>
    </template>
    <template #movements>
      <Movements :movements="movements" @remove="remove" />
    </template>
  </Layout>
</template>

<script setup>
import Layout from "@/components/Layout.vue";
import Header from "@/components/Header.vue";
import Resume from "@/components/Resume/Index.vue";
import Movements from "@/components/Movements/Index.vue";
import Action from "@/components/Action.vue";
import Graphic from "@/components/Resume/Graphic.vue";
import { ref, computed, onMounted } from "vue";

const movements = ref([]);
const label = ref(null);
const amount = ref(null);

const amounts = computed(() => {
  const lastDays = movements.value
    .filter((m) => {
      const today = new Date();
      const oldDate = today.setDate(today.getDate() - 30);
      return m.time > oldDate;
    })
    .map((m) => m.amount);
  console.log(lastDays, amount.value);
  return lastDays.map((m, i) => {
    const lastMovements = lastDays.slice(0, i + 1);
    return lastMovements.reduce((suma, movement) => suma + movement, 0);
  });
});

const totalAmount = computed(() => {
  return movements.value.reduce((suma, m) => suma + m.amount, 0);
});

function create(movement) {
  movements.value.push(movement);
  save();
}
function remove(id) {
  const index = movements.value.findIndex((m) => m.id === id);
  movements.value.splice(index, 1);
  save();
}
function save() {
  localStorage.setItem("movements", JSON.stringify(movements.value));
}

function select(el) {
  amount.value = el;
}

onMounted(() => {
  const localMovements = JSON.parse(localStorage.getItem("movements"));
  if (Array.isArray(localMovements)) {
    movements.value = localMovements.map((m) => {
      return { ...m, time: new Date(m.time) };
    });
  }
});
</script>
