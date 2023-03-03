<template>
  <div class="wrapper">
    <SearchBar />
    <div class="wrapper__cardlist">
      <ProfileCardList :onSelectCard="handleSelectCard" :userData="userList" />
      <div></div>
    </div>
  </div>
</template>

<script setup>
import { reactive, watch } from 'vue'
import { useRoute } from 'vue-router'
import users from '../utils/users.json'
import ProfileCardList from '../components/ProfileCardList.vue'
import SearchBar from '../components/SearchBar.vue'

const userList = reactive(users)
const route = useRoute()

const handleSelectCard = (email) => {
  userList.forEach((user, index) => {
    if (user.email === email) {
      userList[index].selected = !userList[index].selected
      return
    }
  })
}

watch(() => route.query.search, () => {
  userList.forEach((user, index) => {
    userList[index].selected = false
  })
})
</script>

<style lang="scss" scoped>
.wrapper {
  margin: auto;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  padding: 24px 16px;
  gap: 32px;
  width: 721px;
  height: 748px;
  background: #FFFFFF;
  
  &__cardlist {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    padding: 0 20px 1px 0;
    gap: 16px;
    width: 100%;
    flex-grow: 1;
    overflow-y: scroll;
    overflow-x: hidden;
    box-sizing: border-box;
  }
}

::-webkit-scrollbar {
  width: 8px;

  &-thumb {
    background: #ABABAB;
    border-radius: 30px;
  }

  &-track {
    box-shadow: inset 0 0 5px #ababab;
    border-radius: 10px;
    border-left: 3px solid transparent;
    border-right: 3px solid transparent;
  }
}
</style>
