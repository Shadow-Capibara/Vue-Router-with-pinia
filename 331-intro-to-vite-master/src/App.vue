<script setup lang="ts">
import { RouterLink, RouterView, useRoute, useRouter } from 'vue-router'
import { useMessageStore } from '@/stores/message'
import { storeToRefs } from 'pinia'
import { ref } from 'vue'

const store = useMessageStore()
const {message} = storeToRefs(store)
const pageSizes = [1, 2, 4, 6, 8, 10]
const pageSize = ref(pageSizes[1])
const router = useRouter()
const route = useRoute()

const updatePageSize = () => {
  router.push({
    name: 'event-list-view',
    query: { ...route.query, pageSize: pageSize.value, page: 1 }
  })
}
if (route.query.pageSize) {
  pageSize.value = parseInt(route.query.pageSize.toString())
}
</script>

<template>
  <div id="layout">
    <header>
      <div id="flashMessage" v-if="message">
        <h4>{{ message }}</h4>
      </div>
      <div class="wrapper">
        <nav>
          <RouterLink :to="{name: 'event-list-view', query: { pageSize: pageSize }}">Event</RouterLink> |
          <RouterLink :to="{name: 'about'}">About</RouterLink> |
          <RouterLink :to="{name:'Student' }">Student</RouterLink>
        </nav>
        <div>
          <label for="page-size">Event per page</label>
          <select id="page-size" v-model="pageSize" @change="updatePageSize">
            <option v-for="size in pageSizes" :key="size" :value="size">{{ size }}</option>
          </select>
        </div>
      </div>
    </header>
    <RouterView />
  </div>
</template>

<style>

#layout {
  font-family: Avenir, Arial, Helvetica, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;
}

nav a {
 font-weight: bold;
 color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}
h2 {
  font-size: 20px
}

@keyframes yellofade{
  from{
    background-color: yellow;
  }
  to{
    background-color: transparent;
  }
}

#flashMessage{
  animation: yellofade 3s ease-in-out;
}
</style>
