<template>
  <div class="min-h-screen bg-white">
    <div class="max-w-7xl mx-auto p-6">
      
      <!-- Controls -->
      <div class="flex flex-col sm:flex-row gap-4 mb-8">
        <SearchBar v-model:search="searchQuery" />
        <CategoryFilter v-model:category="selectedCategory" :categories="categories" />
        <SortOptions v-model:sort="selectedSort" />
      </div>

      <!-- Products Heading -->
      <h2 class="text-2xl font-bold tracking-tight text-gray-900 border-b-2 border-black pb-3 mb-6">
        Products
      </h2>

      <!-- Error Message -->
      <p v-if="error" class="text-center text-red-500">{{ error }}</p>

      <!-- Product List -->
      <ProductList v-if="!loading && !error" :products="filteredAndSortedProducts" />

      <!-- No products found -->
      <p v-if="!loading && filteredAndSortedProducts.length === 0" class="mt-6 text-center text-gray-500">
        No products found
      </p>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from "vue";
import axios from "axios";
import SearchBar from "./components/SearchBar.vue";
import CategoryFilter from "./components/CategoryFilter.vue";
import SortOptions from "./components/SortOptions.vue";
import ProductList from "./components/ProductList.vue";

// State
const products = ref([]);
const categories = ref([]);
const searchQuery = ref("");
const selectedCategory = ref("");
const selectedSort = ref("");
const loading = ref(false);
const error = ref("");

// Helper to parse price strings
const parsePrice = (s) => {
  if (!s) return 0;
  const digits = String(s).replace(/[^\d]/g, "");
  return Number(digits) || 0;
};

// Fetch products from JSON (Axios)
const fetchProducts = async () => {
  loading.value = true;
  error.value = "";
  try {
    const res = await axios.get("/shoes.json"); // JSON path
    products.value = res.data;

    // Extract unique categories
    categories.value = [...new Set(products.value.map((p) => p.category))];
  } catch (err) {
    error.value = "Failed to load products.";
    console.error(err);
  } finally {
    loading.value = false;
  }
};

// Call fetch on mount
onMounted(fetchProducts);

// Computed: filter + sort
const filteredAndSortedProducts = computed(() => {
  let result = [...products.value];

  const q = searchQuery.value.trim().toLowerCase();
  if (q) {
    result = result.filter((p) =>
      (p.name && p.name.toLowerCase().includes(q)) ||
      (p.category && p.category.toLowerCase().includes(q)) ||
      (p.price && p.price.toLowerCase().includes(q))
    );
  }

  if (selectedCategory.value) {
    result = result.filter((p) => p.category === selectedCategory.value);
  }

  if (selectedSort.value === "az") {
    result.sort((a, b) => a.name.localeCompare(b.name));
  } else if (selectedSort.value === "za") {
    result.sort((a, b) => b.name.localeCompare(a.name));
  } else if (selectedSort.value === "lowHigh") {
    result.sort((a, b) => parsePrice(a.price) - parsePrice(b.price));
  } else if (selectedSort.value === "highLow") {
    result.sort((a, b) => parsePrice(b.price) - parsePrice(a.price));
  }

  return result;
});
</script>
