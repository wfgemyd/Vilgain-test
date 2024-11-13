//todo
/*
+ Create Category Filter Component that will filter products based on categories (from json) must be immediate Filtering (reactive)
+ Reset Filters Button

*/

<script setup lang="ts">
import { defineProps, defineEmits, computed } from 'vue';

interface Category {
  name: string;
  count: number;
}

const props = defineProps<{
  categories: Category[];
  selectedCategories: string[];
}>();

const emits = defineEmits<{
  (e: 'update:selectedCategories', value: string[]): void;
}>();

// Create a computed proxy for selectedCategories
const selectedCategoriesProxy = computed({
  get: () => props.selectedCategories,
  set: (value: string[]) => {
    emits('update:selectedCategories', value);
  },
});

const resetFilters = () => {
  selectedCategoriesProxy.value = [];
};
</script>

<template>
  <div class="category-filter">
    <h2>Filter by Category</h2>
    <div class="category-list">
      <div
          v-for="category in categories"
          :key="category.name"
          class="category-item"
      >
        <label>
          <input
              type="checkbox"
              :value="category.name"
              v-model="selectedCategoriesProxy"
          />
          {{ category.name }} ({{ category.count }})
        </label>
      </div>
    </div>
    <button v-if="selectedCategoriesProxy.length" @click="resetFilters">
      Reset Filters
    </button>
  </div>
</template>

<style scoped>
.category-filter {
  margin-bottom: 20px;
  text-align: center;
}

.category-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 10px;
}

.category-item {
  margin: 5px;
}

button {
  margin-top: 10px;
  padding: 8px 12px;
  background-color: var(--vt-c-indigo);
  color: var(--vt-c-white);
  border: none;
  cursor: pointer;
  font-size: 14px;
}
</style>
