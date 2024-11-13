//todo
/*
+ Display Products as Tiles: aktin.cz
*/

<template>
  <div class="product-tile">
    <img :src="product.image" :alt="product.title" class="product-image" />
    <div class="product-info">
      <h2 class="product-title">{{ product.title }}</h2>
      <p class="product-price">${{ product.price }}</p>
      <p class="product-description">{{ product.description }}</p>
    </div>
  </div>
</template>

<script setup lang="ts">
import { defineProps } from 'vue';
import { Product } from '../types/types';

const props = defineProps<{
  product: Product; //this component (ProductTile) accepts a prop named product of type Product
}>();
</script>

<style scoped>
.product-tile {
  width: 180px;
  height: 374px;
  border: 1px solid var(--color-border);
  background-color: var(--color-background-soft);
  color: var(--color-text);
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  align-items: center;
  transition: transform 0.2s, box-shadow 0.2s;
}

.product-tile:hover {
  transform: translateY(-5px);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.product-image {
  display: flex;
  width: fit-content;
  max-width: 160px;
  height: 162.5px;
  object-fit: cover;
}

.product-info {
  width: 100%;
  height: 203.5px;
  padding: 10px;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.product-title {
  font-size: 14px;
  font-weight: bold;
  color: var(--color-heading);
  margin-bottom: 5px;
}

.product-price {
  font-size: 16px;
  color: var(--vt-c-indigo);
  margin-bottom: 5px;
}

.product-description {
  font-size: 12px;
  color: var(--color-text);
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 5;
  -webkit-box-orient: vertical;
}
</style>
