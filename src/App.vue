<script setup>
import Main from './components/Main.vue';
</script>

<template>
  <div>
    <nav class="blue lighten-2">
      <div class="nav-inner">
        <img class="logo" src='./assets/Rick_and_Morty.svg' alt="">
        <div class="right code">
          <a href="https://github.com/Alexander814Yakovlev/rickAndMortyVue" target="_blank"><i class="material-icons">code</i></a>
        </div>
      </div>

    </nav>
  </div>
  <Main></Main>
  <footer class="page-footer blue">
    <div class="footer-copyright">
      <div class="container">
        © 2024
        <a class="grey-text text-lighten-4 right" href="https://t.me/alexander_yakovlev_814">Alexander Yakovlev</a>
      </div>
    </div>
  </footer>
</template>

<style scoped></style>
