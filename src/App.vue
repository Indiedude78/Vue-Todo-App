<script setup>
import Header from "./components/header.vue";
import Counter from './components/counter.vue'

import Nav from './components/nav.vue'

</script>

<template>
  <div>
    <Nav />
  </div>
  <router-view />
  <!-- <Header title="Gotta Do" />
  <Todo /> -->

</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(10) / 2);
  }
}
</style>
