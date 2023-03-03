<template>
  <ProfileCard
    v-for="user in filteredData"
    :onSelect="onSelectCard"
    :data="user"
    :key="user.email"
    :highlight="searchStr"
  />
  <div ref="loadMore" v-if="hasMoreData"></div>
</template>

<script setup>
import { computed, ref, defineProps, onMounted, watch } from 'vue'
import { useRoute } from 'vue-router'
import ProfileCard from './ProfileCard.vue'

const props = defineProps({
  userData: Array,
  onSelectCard: Function
})

const route = useRoute()
const searchStr = ref('')
const hasMoreData = ref(true)
const currentPage = ref(1)
const pageSize = ref(10)
const observer = ref(null)
const loadMore = ref(null);

const filteredData = computed(() => 
  props.userData
    .filter(user => (
      user.name.first.toLowerCase().indexOf(searchStr.value.toLowerCase()) !== -1 ||
      user.name.last.toLowerCase().indexOf(searchStr.value.toLowerCase()) !== -1 ||
      user.email.toLowerCase().indexOf(searchStr.value.toLowerCase()) !== -1 ||
      user.address.toLowerCase().indexOf(searchStr.value.toLowerCase()) !== -1 ||
      user.country.toLowerCase().indexOf(searchStr.value.toLowerCase()) !== -1
    ))
    .slice(0, Math.min(currentPage.value * pageSize.value, props.userData.length))
)

onMounted(() => {
  searchStr.value = route.query.search || ''
  observer.value = new IntersectionObserver(entries => {
    if (entries[0].isIntersecting) {
      if (filteredData.value.length >= props.userData.length) {
        observer.value.disconnect()
        hasMoreData.value = false
      } else {
        currentPage.value ++
      }
    }
  });
  observer.value.observe(loadMore.value);
})

watch(() => route.query.search, (newSearchStr) => {
  searchStr.value = newSearchStr || ''
  currentPage.value = 1
})
</script>

<style lang="scss" scoped>
</style>
