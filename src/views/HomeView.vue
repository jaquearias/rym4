<template>
  <div class="home">
    <ListaPersonajes msg="Welcome to Your Vue.js App"/>
    <!-- <SideBar /> -->
  </div>
</template>

<script>
// @ is an alias to /src
import ListaPersonajes from '@/components/ListaPersonajes.vue'
// import SideBar from '@/components/SideBar.vue'

export default {
  name: 'HomeView',
  components: {
    ListaPersonajes
  }
}
</script>
