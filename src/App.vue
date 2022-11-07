<template>
  <NavbarList />
  <router-view/>
</template>

<script>
import NavbarList from './components/NavbarList.vue'
export default {
  name: "App",
  components:{
    NavbarList,

  },
}
</script>

<style>

</style>
