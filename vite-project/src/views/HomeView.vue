<template>
  <div class="main-page">
    <div class="primary-information">
      <h1>Prominent RF Political / Business Figures:</h1>
      <div class="CreatedPoliticianCard">
        <div v-for="(Politician, index) in wealthyRussians" :key="index" class="PoliticianData">
          <div class="CreatedPoliticianCardImgContainer">
            <img class="PoliticianImg" :src="Politician.img" :alt="Politician.name">
          </div>
          <div class="CreatedPoliticianCardInfoContainer">
            <h2>{{ Politician.name }}</h2>
            <p> {{ Politician.type }}</p>
            <p v-if="Politician.networth === undefined">Asset Value: Unknown</p>
            <p v-else>Asset Value: ${{ Politician.networth }}</p>
            <button @click="confiscateAssets(Politician)">Confiscate Assets</button>
          </div>
        </div>
      </div>
    </div>
    <div class="checkout">
    <p>Total Confiscated: {{ formattedConfiscatedAmount }}</p>
    ... other checkout elements ...
    </div>
  </div>
</template>

<script>
import { ArrayOfRussianCorruptPoliticians } from '@/stores/wealthyRussians.js'; 

export default {
  data() {
    return {
      wealthyRussians: ArrayOfRussianCorruptPoliticians,
      confiscatedAmount: 0,
    };
  },
  methods: {
    confiscateAssets(politician) {
      console.log('Stage 1 complete')
      if (politician.networth > 0) {
        this.confiscatedAmount += politician.networth;
        politician.networth = 0;
        this.playConfiscateSound(); // Play the sound after confiscation
      }
    },
    playConfiscateSound() {
      console.log('stage 2 complete')
      const audio = new Audio("vite-project/src/assets/mp3files/boowomp.mp3"); // Replace with actual path
      audio.play();
    },
  },
  computed: {
    formattedConfiscatedAmount() {
      // Convert amount to billions
      const billions = this.confiscatedAmount / 1e9;
      // Format with currency symbol and two decimal places
      return `$${billions.toFixed(2)} Billion`;
    },
  },
};
</script>

<style scoped>
.main-page {
  display: flex;
}
.primary-information {
  width: 70vw;
}
.checkout {
  width: 30vw;
  margin: 1rem;
  padding: 2rem;
}
.main-page {
  max-width: 100%;
  margin: 0 auto;
  padding: 2rem;
}

.CreatedPoliticianCard {
  display: flex;
  flex-wrap:wrap;
  gap: 2rem;
}

.PoliticianData {
  border-radius: .7rem;
  overflow: hidden;
  flex: 1 1 20rem;
  max-width: 25rem;
  border-color: #234234;
}

.CreatedPoliticianCardImgContainer {
  height: 16rem;
  overflow: hidden;
}

.PoliticianImg {
  width: 100%;
  height: auto;
}

.CreatedPoliticianCardInfoContainer {
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
