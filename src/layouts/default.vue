<template>
  <div>
    <Navbar/>
    <Nuxt />
  </div>
</template>

<script>
import Navbar from "@/components/navbar/Navbar.vue"

export default{
  components:{
    Navbar
  }
}
</script>


<style>
@import url('https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;700&display=swap');
html {
  margin: 0;
  padding: 0;
  background-color: #e6f3f7;
  font-family: 'DM Sans', sans-serif;
}
body {
  margin: 0;
}
</style>
