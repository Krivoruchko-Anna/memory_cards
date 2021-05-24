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
  background: url('https://media.istockphoto.com/vectors/computer-circuit-board-vector-id1132382067?k=6&m=1132382067&s=612x612&w=0&h=3I6pXHj9dMQM6493CEI3bqZnycDNk2Wm9V4c3o82Te0=');
}
</style>
