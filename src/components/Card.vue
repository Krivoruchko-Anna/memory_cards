<template>
  <div class="card" :class="{ 'hidden': card.backSide,  'animated': card.animated }">
    {{ !card.backSide ? card.number :  '' }}
  </div>
</template>

<script>
export default {
  props: ['card']
}
</script>

<style lang="scss">
.card {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 60px;
  height: 60px;
  padding: 10px;
  margin: 5px;
  cursor: pointer;
  border-radius: 4px;
  color: white;
  background-color: #94b7dc;
  transition: .3s all ease-in-out;

  @media (max-width: 500px) {
    width: 50px;
    height: 50px;
  }

  &:hover {
    background-color: #96b1d2;
  }
}

.hidden {
  background-color: #7391b1;
  background-size: cover;
  transition: .4s all ease-in-out;
}

.animated {
  animation: flipCard .4s ease-in-out;
  transition: .3s all ease-in-out;
}

@keyframes flipCard {
  0% {
    transform: perspective(120px) rotateX(0deg) rotateY(0deg);
    color: transparent;
  }
  94% {
    color: transparent;
  }
  100% {
    transform: perspective(120px) rotateX(-180deg) rotateY(-179.9deg);
    color: white;
  }
}
</style>
