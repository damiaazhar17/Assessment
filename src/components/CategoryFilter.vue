<template>
  <div class="flex-1">
    <select
      :value="category"
      @change="$emit('update:category', $event.target.value)"
      class="w-full rounded-xl border border-gray-300 px-4 py-2 shadow-sm focus:border-stone-500 focus:ring-2 focus:ring-stone-400 outline-none"
    >
      <option value="">All Categories</option>
      <option
        v-for="cat in categories"
        :key="cat"
        :value="cat"
      >
        {{ cat }}
      </option>
    </select>
  </div>
</template>

<script setup>
defineProps({
  category: String,
  categories: Array
})
</script>
