<template>
  <div class="navbar">
    <div class="container p-5 mx-auto">
      <router-link :to="{ name: 'Home' }" class="text-2xl">Sarunk</router-link>
    </div>
  </div>
</template>

<script>
export default {};
</script>

<style scoped>
.navbar {
  background-color: #51c4d3;
}
a {
  color: #faf3f3;
}
</style>