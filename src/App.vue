<script setup>

import Scheduler from './components/Scheduler.vue';
</script>

<template>
  
  <main>
    <Scheduler />
  </main>
</template>

<style>
@import './assets/base.css';

#app {
  font-weight: normal;
}
body{
    background-color: #FAF9F9;

}

header {
  line-height: 1.5;
}

.logo {
  display: block;
}

a,
.green {
  text-decoration: none;
  color: hsla(160, 100%, 37%, 1);
  transition: 0.4s;
}

@media (hover: hover) {
  a:hover {
    background-color: hsla(160, 100%, 37%, 0.2);
  }
}

@media (min-width: 1024px) {


  
}
</style>
