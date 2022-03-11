<script setup lang="ts">
import { ref } from "vue";
import InputSelectOld from "./components/Inputs/InputSelectOld.vue";
import InputSelect from "./components/Inputs/InputSelect.vue";

const filterData = ref([
  {
    label: "Employee",
    options: ["AHOL", "AH", "AHUT", "ASET", "ABAJ", "dolor", "sit"],
  },
  {
    label: "Customer",
    options: ["Customer 1", "Lorem", "Dolor", "Ipsum", "Amet"],
  },
  { label: "Project", options: [] },
]);
</script>

<template>
  <div class="center">
    <InputSelect
      v-for="select in filterData"
      :label="select.label"
      :options="select.options"
      :key="select.label"
    />
  </div>
</template>

<style>
@import "./assets/base.css";

.center {
  display: flex;
  gap: 1rem;
  padding: 4rem;
}
</style>
