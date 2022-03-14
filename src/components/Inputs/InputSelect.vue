<script setup lang="ts">
import { ref, computed, watchEffect } from "vue";

// icons
import IconChevron from "../../icons/IconChevron.vue";
import IconUndo from "../../icons/IconUndo.vue";

// define props
interface Select {
  label: string;
  options: string[];
}
const props = defineProps<Select>();

// data
const optionsMenuOpen = ref(false);
const selectedOption = ref("");
const searchTerm = ref("");
const optionsMenu = ref();
const searchInputRef = ref();
const optionsList = ref();

// methods
const toggleOptions = () => {
  optionsMenuOpen.value = !optionsMenuOpen.value;
};

const closeOptions = () => {
  optionsMenuOpen.value = false;
};

const chooseOption = (option: string) => {
  selectedOption.value = option;
  optionsMenuOpen.value = false;
};

const resetInput = () => {
  selectedOption.value = "";
};

const focusToTop = () => {
  optionsList.value.scrollTop = 0;
  searchInputRef.value.focus();
};

// close on click outside of element
const handleClickOutside = (e: Event) => {
  if (optionsMenu.value && !optionsMenu.value.contains(e.target)) {
    optionsMenuOpen.value = false;
  }
};
watchEffect(() => {
  document.addEventListener("click", handleClickOutside, true);
  return () => {
    document.removeEventListener("click", handleClickOutside, true);
  };
});

// computed
const filteredOptions = computed(() =>
  searchTerm.value
    ? props.options.filter((option) =>
        option.toLowerCase().includes(searchTerm.value.toLowerCase())
      )
    : props.options
);

// directives
const vFocus = {
  mounted: (el: HTMLElement) => el.focus(),
};
</script>

<template>
  <div class="select" @keydown.escape="closeOptions" ref="optionsMenu">
    <p v-if="selectedOption" class="label">{{ props.label }}</p>
    <button @click="resetInput" v-if="selectedOption" class="reset-input">
      <IconUndo />
    </button>
    <button
      @click="toggleOptions"
      class="select-header"
      :class="{ selected: selectedOption, disabled: !props.options.length }"
      :disabled="!props.options.length"
    >
      {{ selectedOption || props.label }}
      <IconChevron :class="{ rotate: optionsMenuOpen }" />
    </button>
    <div v-if="optionsMenuOpen" class="options-container" ref="optionsList">
      <input
        v-model="searchTerm"
        type="text"
        placeholder="Search"
        autocomplete="off"
        v-focus="true"
        ref="searchInputRef"
      />
      <ul class="options-list">
        <li
          v-for="(option, index) in filteredOptions"
          :key="option"
          v-on="{
            focusout: index === filteredOptions.length - 1 ? focusToTop : null,
          }"
        >
          <button @click="chooseOption(option)">{{ option }}</button>
        </li>
      </ul>
    </div>
  </div>
</template>

<style>
.select {
  display: inline-block;
  position: relative;
}

.label {
  background-color: var(--light-100);
  font-size: 0.8rem;
  position: absolute;
  top: -0.7rem;
  left: 0.2rem;
  z-index: 20;
}

.reset-input {
  background-color: transparent;
  border: none;
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  bottom: 3rem;
  right: -1rem;
}

.reset-input:hover {
  background-color: var(--light-200);
}

.reset-input:focus-visible {
  background-color: var(--light-200);
  outline: none;
}

.reset-input svg {
  fill: var(--yellow-500);
  height: 1.5rem;
  width: 1.5rem;
}

.select-header {
  background-color: var(--light-50);
  border: 1px solid var(--light-300);
  height: 2.5rem;
  padding-inline: 0.5rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.select-header svg {
  margin-left: 0.3rem;
  height: 1rem;
  width: 1rem;
}

.select-header.selected {
  background-color: var(--light-100);
}

.select-header.disabled {
  background-color: var(--light-100);
  color: var(--light-500);
}

.select-header.disabled svg {
  fill: var(--light-500);
}

.options-container {
  max-height: 10rem;
  overflow-y: auto;
  position: absolute;
  top: 3rem;
  z-index: 10;
}

.options-container input {
  background-color: var(--light-50);
  border: 1px solid var(--light-300);
  padding: 0.5rem;
  position: sticky;
  top: 0;
  min-width: 100%;
  max-width: 100%;
  z-index: 12;
}

.options-container input:hover {
  background-color: var(--light-200);
}

.options-container input:focus {
  border: 1px solid var(--blue-400);
  outline: none;
}

.options-list {
  background-color: var(--light-50);
  border: 1px solid var(--light-300);
  border-top: none;
}

.options-list button {
  background-color: transparent;
  border: none;
  text-align: left;
  padding: 0.5rem;
  width: 100%;
}

.options-list li:not(:last-child) {
  border-bottom: 1px solid var(--light-300);
}

.options-list button:hover {
  background-color: var(--light-200);
}

.options-list button:focus-visible {
  background-color: var(--light-200);
  outline: none;
}

.rotate {
  transform: rotate(180deg);
}
</style>
