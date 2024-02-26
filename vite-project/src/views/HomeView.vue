<template>
  <div class="main-page">
    <div class="primary-information">
      <h1 class="politicians-heading">Prominent RF Political / Business Figures:</h1>
      <div class="CreatedPoliticianCard">
        <div v-for="(russianScumbag, index) in wealthyRussians" :key="index" class="PoliticianData">
          <div class="CreatedPoliticianCardImgContainer">
            <img class="PoliticianImg" :src="russianScumbag.img" :alt="russianScumbag.name">
          </div>
          <div class="CreatedPoliticianCardInfoContainer">
            <h2>{{ russianScumbag.name }}</h2>
            <p> {{ russianScumbag.type }}</p>
            <p>Rich Guy's Worth: {{ russianScumbag.riches ? `$${twoPlaceDeciChanger(russianScumbag.riches)}` : 'Man\'s a Brokie' }}</p> 
            <button @click="russianScumbag.confiscated ? returnAssets(russianScumbag) : confiscateAssets(russianScumbag)">{{ russianScumbag.confiscated ? 'Return Confiscated Assets' : 'Confiscate Russian Assets' }}</button>
          </div>
        </div>
      </div>
    </div>
    <div class="ConfiscatedAssetsCard">
      <h1>Seized Property / Funds / Stocks</h1>
      <div v-if="politicianConfiscatedAssetsList.length === 0">Get to work, can't be letting these corrupt Russian businessmen and politicians take over the world! Too much time wasted don't you think?!</div>
      <div v-else>
        <div v-for="(item, index) in politicianConfiscatedAssetsList" :key="index">
          <p>{{ item.name }} - ${{ twoPlaceDeciChanger(item.riches) }}</p>
          <button @click="removeFrompoliticianConfiscatedAssetsList(index)">Remove</button>
        </div>
        <p>Seized: {{ formattedConfiscatedAmount }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import { ArrayOfRussianCorruptPoliticians } from '@/stores/wealthyRussians.js'; 
export default {
  data() {
    return {
      wealthyRussians: ArrayOfRussianCorruptPoliticians,
      politicianConfiscatedAssetsList: [],
      confiscatedAmount: 0,
    };
  },
  methods: {
    confiscateAssets(russianScumbag) {
      if (russianScumbag.riches > 0 && !russianScumbag.confiscated) {
        this.confiscatedAmount += russianScumbag.riches;
        this.politicianConfiscatedAssetsList.push({ name: russianScumbag.name, riches: russianScumbag.riches });
        russianScumbag.confiscated = true;
        russianScumbag.originalNetworth = russianScumbag.riches;
        russianScumbag.riches = 0;
        this.playConfiscateSound();
      }
    },
    returnAssets(russianScumbag) {
      const index = this.politicianConfiscatedAssetsList.findIndex(item => item.name === russianScumbag.name);
      if (index !== -1) {
        this.confiscatedAmount -= this.politicianConfiscatedAssetsList[index].riches;
        this.politicianConfiscatedAssetsList.splice(index, 1);
        russianScumbag.riches = russianScumbag.originalNetworth;
        russianScumbag.confiscated = false;
      }
    },
    playConfiscateSound() {
      const sound = new Howler.Howl({
        src: "vite-project/public/mp3files/boowomp.mp3", 
      });
      sound.play();
    },
    twoPlaceDeciChanger(value) {
      return new Intl.NumberFormat('en-US', { 
        style: 'decimal', 
        minimumFractionDigits: 2, 
        maximumFractionDigits: 2 
      }).format(value);
    },
    removeFrompoliticianConfiscatedAssetsList(index) {
      const item = this.politicianConfiscatedAssetsList[index];
      this.confiscatedAmount -= item.riches;
      const russianScumbag = this.wealthyRussians.find(russianScumbag => russianScumbag.name === item.name);
      if (russianScumbag) {
        russianScumbag.riches = russianScumbag.originalNetworth;
        russianScumbag.confiscated = false;
      }
      this.politicianConfiscatedAssetsList.splice(index, 1);
    },
  },
  computed: {
    formattedConfiscatedAmount() {
      const billions = this.confiscatedAmount / 1e9;
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
  width: 60vw;
}
.ConfiscatedAssetsCard {
  width: 20vw;
  margin: 1rem;
  padding: 1rem;
  border-color: red;
  border-width: 20px;
}

.CreatedPoliticianCard {
  display: flex;
  flex-wrap:wrap;
  gap: 2rem;
}

.PoliticianData {
  border-radius: .7rem;
  border-color: red;
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
