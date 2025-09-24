<template>
  <div class="relative flex-1">
    <input
      type="text"
      :value="search"
      @input="$emit('update:search', $event.target.value)"
      placeholder="What's on your mind?"
      class="w-full rounded-xl border border-gray-300 px-4 py-2 pl-10 shadow-sm focus:border-stone-500 focus:ring-2 focus:ring-stone-400 outline-none"
    />
    <!-- Search icon -->
    <svg xmlns="http://www.w3.org/2000/svg"
         class="absolute left-3 top-2.5 h-5 w-5 text-gray-400"
         fill="none" viewBox="0 0 24 24" stroke="currentColor">
      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
            d="M21 21l-4.35-4.35M9.5 17A7.5 7.5 0 109.5 2a7.5 7.5 0 000 15z"/>
    </svg>
  </div>
</template>

<script setup>
defineProps({
  search: String
})
</script>
