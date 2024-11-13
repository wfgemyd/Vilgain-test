//todo
/*
+ Load Products from API : https://fakestoreapi.com/products
+ Use Fetch API with Async/Await
-----after product tile creation-------
+ Display Products as Tiles (loop)
-----after cat filter-------
+ Manage URL History
*/


<script setup lang="ts">
import {ref, computed, onMounted, watch} from 'vue';
import ProductTile from './ProductTile.vue';
import CategoryFilter from './CategoryFilter.vue';
import {Product, Category} from '../types/types';

const products = ref<Product[]>([]);
const categories = ref<Category[]>([]);
const selectedCategories = ref<string[]>([]);

const updateURLWithFilters = (filters: string[]) => {
  const url = new URL(window.location.href);
  if (filters.length > 0) {
    url.searchParams.set('categories', filters.join(','));
  } else {
    url.searchParams.delete('categories');
  }
  window.history.replaceState({}, '', url.toString()); // Update the URL without refreshing the page
};

const initializeFiltersFromURL = () => {
  const urlParams = new URLSearchParams(window.location.search);
  const categoriesParam = urlParams.get('categories');
  if (categoriesParam) {
    const availableCategories = categories.value.map((c) => c.name); // Get available categories
    selectedCategories.value = categoriesParam
        .split(',')
        .filter((cat) => availableCategories.includes(cat));
  }
};

const fetchProducts = async () => { //fetches product data from an external API and initializes the app

  try {
    const response = await fetch('https://fakestoreapi.com/products');
    const data: Product[] = await response.json();
    products.value = data;
    processCategories();
    initializeFiltersFromURL(); // Ensure filters are initialized after categories are processed
  } catch (error) {
    console.error('Error fetching products:', error);
  }
};

const processCategories = () => {
  const categoryCount: Record<string, number> = {}; // Count the number of products in each category

  products.value.forEach((product) => { // Loop through products to count categories
    const category = product.category;
    if (categoryCount[category]) {
      categoryCount[category]++;
    } else {
      categoryCount[category] = 1;
    }
  });

  const categoryArray = Object.entries(categoryCount).map(([name, count]) => ({
    name,
    count: count as number,
  })); // Convert the object to an array of Category objects

  categories.value = categoryArray.sort((a, b) => b.count - a.count); // Sort categories by count
};

const filteredProducts = computed(() => { //updates reactively whenever products or selectedCategories change.
  if (selectedCategories.value.length === 0) {
    return products.value;
  }
  return products.value.filter((product) =>
      selectedCategories.value.includes(product.category)
  );
});

// watch for changes in selectedCategories to update the URL
watch(
    selectedCategories,
    (newSelected) => {
      updateURLWithFilters(newSelected);
    },
    {immediate: true}
);

onMounted(() => {
  // fetch products when the component is mounted
  fetchProducts();
});
</script>

<template>
  <div class="product-list-container">
    <h1>Product List</h1>
    <!-- Use v-model for two-way binding -->
    <CategoryFilter
        :categories="categories"
        v-model:selectedCategories="selectedCategories"
    />

    <div class="product-list" v-if="filteredProducts.length">
      <ProductTile
          v-for="product in filteredProducts"
          :key="product.id"
          :product="product"
      />
    </div>
    <p v-else>No products found.</p>
  </div>
</template>

<style scoped>
.product-list-container {
  max-width: 800px;
  margin: 0 auto;
  text-align: center;
}

.product-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 20px;
}

h1 {
  margin-bottom: 20px;
  color: var(--color-heading);
}
</style>
