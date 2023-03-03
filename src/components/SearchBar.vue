<template>
  <div class="search">
    <img class="search__icon" :src="searchIcon" alt="search-icon" />
    <input class="search__input" v-model="searchStr" />
  </div>
</template>

<script setup>
import { ref, watch, onMounted } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import { debounce } from 'lodash'
import searchIcon from '../assets/search-icon.svg'

const router = useRouter()
const searchStr = ref('')
const route = useRoute()

onMounted(() => {
  if (route.query.search) {
    searchStr.value = route.query.search
  }
})

watch(searchStr, debounce((newSearchStr) => {
  router.push({ query: { search: newSearchStr } })
}, 500))

watch(() => route.query.search, (newSearchStr) => {
  searchStr.value = newSearchStr
})
</script>

<style lang="scss" scoped>
.search {
  width: 100%;
  position: relative;

  &__input {
    background-color: #EEEEEE;
    outline: none;
    border: none;
    border-radius: 8px;
    width: 100%;
    height: 51px;
    font-size: 21px;
    color: #555555;
    padding-left: 50px;
    box-sizing: border-box;
  }

  &__icon {
    position: absolute;
    left: 10px;
    top: 13px;
  }
}
</style>