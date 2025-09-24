<template>
  <div class="flex-1">
    <select
      :value="sort"
      @change="$emit('update:sort', $event.target.value)"
      class="w-full rounded-xl border border-gray-300 px-4 py-2 shadow-sm focus:border-stone-500 focus:ring-2 focus:ring-stone-400 outline-none"
    >
      <option value="">Default</option>
      <option value="az">Name: A → Z</option>
      <option value="za">Name: Z → A</option>
      <option value="lowHigh">Price: Low to High</option>
      <option value="highLow">Price: High to Low</option>
    </select>
  </div>
</template>

<script setup>
defineProps({
  sort: String
})
</script>
