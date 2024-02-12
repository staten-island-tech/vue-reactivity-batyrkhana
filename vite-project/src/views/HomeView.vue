<template>
  <div class="main-page">
    <h1>Prominent RF Political / Business Figures:</h1>
    <div class="cardPolit">
      <div v-for="(disgrace, index) in wealthyRussians" :key="index" class="person">
        <div class="disgraceImgContainer">
          <img class="corrupt" :src="disgrace.img" :alt="disgrace.name">
        </div>
        <div class="infoPolit">
          <h2>{{ disgrace.name }}</h2>
          <p> {{ disgrace.type }}</p>
          <p v-if="disgrace.networth === undefined">Asset Value: Unknown</p>
          <p v-else>Asset Value: ${{ disgrace.networth }}</p>
          <button @click="confiscateAssets(disgrace)">Confiscate Assets</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { rusPolitArr } from '@/stores/wealthyRussians.js'; 

export default {
  data() {
    return {
      wealthyRussians: rusPolitArr
    };
  },
  methods: {
    confiscateAssets(politician) {
      if (politician.networth !== undefined) {
        politician.networth = 0;
      }
    }
  }
};
</script>

<style scoped>
.main-page {
  max-width: 80rem;
  margin: 0 auto;
  padding: 2rem;
}

.cardPolit {
  display: flex;
  flex-wrap:wrap;
  gap: 2rem;
}

.person {
  border-radius: .7rem;
  overflow: hidden;
  flex: 1 1 20rem;
  max-width: 25rem;
  border-color: #234234;
}

.disgraceImgContainer {
  height: 13rem;
  overflow: hidden;
}

.corrupt {
  width: 100%;
  height: auto;
}

.infoPolit {
  padding: 2rem;
  color: black;
}

button {
  color: #fff;
  border: none;
  margin-top: .5rem;
  padding: .4rem 16px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #ff8000;
}
h1{
  margin: 1rem;
}
</style>
