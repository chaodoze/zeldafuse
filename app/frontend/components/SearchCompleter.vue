<template>
<div :class="{'search-active':hasFocus}">
  <form class="search-dropdown float-right mt-md-0 mt-3"  >
    <div>
        <input ref="searchbox" v-model="searchText" @focus="hasFocus=true" @keyup="fetchSuggestions" class="form-control form-control-sm" type="text" placeholder="Search Authors"
        aria-label="Search" style="font-size:16px;">
      <div class="col-auto" v-if="hasFocus"><a @click="defocus" class="btn btn-xsm btn-outline-secondary">cancel</a></div>

    </div>
    <div v-if="hasFocus" class="search-dropdown-menu">
      <div class="form-group search-card">
        <div v-if="!showSuggestions">       
          <div class="text-muted px-3 py-3">Try searching for authors by name or Twitter handle</div>
        </div>
        <SearchSuggestion v-show="showSuggestions" v-for="suggestion in suggestions" @subscribed="defocus">
          <template #item>
            <slot name="item" :item=suggestion></slot>
          </template>
        </SearchSuggestion>
      </div>
    </div>
  </form>
  <div class="search-overlay"></div>
</div>
</template>

<script setup>
  const props = defineProps({
    fetchSuggestions: Function,
  })
  import {ref, reactive, computed} from 'vue'
  import SearchSuggestion from './SearchSuggestion.vue'
  const hasFocus = ref(false)
  const searchText = ref('')
  const suggestions = reactive([])  
  const showSuggestions = computed(
    ()=>hasFocus.value && suggestions.length > 0
  )

  function defocus() {
    if (hasFocus.value) {
      hasFocus.value = false
    }
  }
  async function fetchSuggestions() {
    hasFocus.value = true
    if (searchText.value.length < 3) {
      return
    }
    const newSuggestions = await props.fetchSuggestions(searchText.value)
    console.log('got new sugg', newSuggestions, suggestions)
    suggestions.length = 0
    suggestions.push(...newSuggestions)
  }
</script>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>
