<template>
  <SearchCompleter :fetchSuggestions="fetchSuggestions">
    <template #item="{item}">
      hello world
      <span>{{item.value}}</span>
    </template>
  </SearchCompleter>
</template>

<script setup>
  import SearchCompleter from './SearchCompleter.vue'

  const fetchSuggestions = ()=>{
    console.log('fetching suggestions')
    return [{value:1},{value:2},{value:3},{value:4}]
  }
</script>

<style scoped>
</style>
