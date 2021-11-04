<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/Logo-Web.png">
    <HelloWorld msg="Welcome to the Emergency Training Front End Portal"/>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'Home',
  components: {
    HelloWorld
  }
}
</script>
