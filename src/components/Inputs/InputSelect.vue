<script setup lang="ts">
import { ref, computed, nextTick } from "vue";
import IconChevron from "../../icons/IconChevron.vue";
import IconUndo from "../../icons/IconUndo.vue";

const label = ref("Select");
const selectedOption = ref("");
const isOpen = ref(false);
const searchTerm = ref("");
const options = ref(["Option 1", "Hello", "World"]);
const searchInput = ref();

const filterOptionsInput = ref<null | { isOpen: () => null }>(null);

const toggleOptions = async () => {
  isOpen.value = !isOpen.value;

  await nextTick();
  if (isOpen.value) {
    searchInput.value.focus();
  } else {
    searchInput.value.blur();
  }
};

const pickOption = (option: string) => {
  selectedOption.value = option;
  isOpen.value = false;
};

const resetOption = () => {
  selectedOption.value = "";
};

const filteredList = computed(() =>
  searchTerm.value
    ? options.value.filter((option) =>
        option.toLowerCase().includes(searchTerm.value.toLowerCase())
      )
    : options.value
);
</script>

<template>
  <div class="select-wrapper">
    <p v-if="selectedOption" class="label">
      {{ label }}
      <button @click="resetOption" class="btn-icon" aria-label="reset">
        <IconUndo />
      </button>
    </p>
    <button @click="toggleOptions" class="select-label">
      {{ selectedOption || label }} <IconChevron :class="{ rotate: isOpen }" />
    </button>
    <div v-show="isOpen" class="options-container">
      <input
        v-model="searchTerm"
        type="text"
        name="search"
        id="search"
        placeholder="Search"
        ref="searchInput"
      />
      <ul>
        <li v-for="option in filteredList" :key="option">
          <button @click="pickOption(option)" ref="filterOptionsInput">
            {{ option }}
          </button>
        </li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.select-wrapper {
  position: relative;
  width: min(16rem, 90%);
}

.label {
  display: flex;
  justify-content: space-between;
  align-items: center;
  position: absolute;
  top: -2rem;
  width: 100%;
}

.label svg {
  cursor: pointer;
  height: 1.5rem;
  width: 1.5rem;
}

.label svg:hover {
  background-color: var(--light-200);
}

.select-label {
  background-color: var(--light-200);
  border: none;
  cursor: pointer;
  padding: 0.5rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
}

.select-label:hover {
  background-color: var(--light-300);
}

.select-label:focus-visible {
  background-color: var(--light-300);
  outline: none;
}

.select-label svg {
  height: 1rem;
  width: 1rem;
}

.select-label svg.rotate {
  transform: rotate(180deg);
}

.options-container {
  background-color: var(--light-50);
  border: 1px solid var(--light-200);
  position: absolute;
  top: 2.5rem;
  left: 0;
  width: 100%;
}

.options-container input {
  border: 1px solid var(--light-200);
  display: block;
  padding: 0.5rem;
  width: 100%;
}

.options-container input:focus {
  border: 1px solid var(--blue-50);
  outline: none;
}

.options-container li button {
  background-color: transparent;
  border: none;
  cursor: pointer;
  padding: 0.5rem;
  display: block;
  width: 100%;
  text-align: left;
}

.options-container li button:hover {
  background-color: var(--light-200);
}
.options-container li button:focus-visible {
  background-color: var(--light-200);
  outline: none;
}

.btn-icon {
  background-color: transparent;
  border: none;
  border-radius: 5px;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 2px;
}

.btn-icon:hover {
  background-color: var(--light-200);
}

.btn-icon:focus-visible {
  background-color: var(--light-200);
  outline: none;
}
</style>
