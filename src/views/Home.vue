<template>
  <div class="home">
    <app-cards></app-cards>
  </div>
</template>

<script>
import AppCards from "../components/Cards";

export default {
  name: 'Home',
  components: {
    AppCards
  }
}
</script>

<style lang="scss">
.home {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}
</style>
