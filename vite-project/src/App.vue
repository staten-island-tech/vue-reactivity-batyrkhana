<script setup>
import { RouterLink, RouterView } from 'vue-router'

</script>

<template>
  <header>
    <div class="wrapper">
      <div class="announcement">Welcome to my Game! Your job is to find all of the corrupt politicians and send them to prison! You will be tasked with finding finding evidence to support the prosecution against the politicians. Collect the evidence to restore fair rule.</div>
    </div>
  </header>

  <RouterView />
</template>

<style scoped></style>
