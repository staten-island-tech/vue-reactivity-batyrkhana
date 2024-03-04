<template>
  <header>
    <div class="wrapper">
      <h1 class="mainTitle">Asset Confiscator Software</h1>
    </div>
  </header>
  <div class="view">
    <router-view />
  </div>
</template>

<script setup>
import { RouterView } from 'vue-router';
</script>

<style>
  .mainTitle {
    font-size: var(--h1);
    justify-content: center;
    padding-top: 1rem;
    padding-left: 2rem;
    padding-right: 2rem;
    padding-bottom: 0.5rem;
    margin: 1rem;
    background-color: antiquewhite;
  }
</style>
