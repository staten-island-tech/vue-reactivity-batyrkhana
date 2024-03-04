<template>
  <div class="main-page">
    <div class="politicians">
      <h1 class="politicians-main-heading">Prominent RF Political / Business Figures:</h1>
      <div class="politician-main-createdCard">
        <div v-for="(russianScumbag, index) in wealthyRussians" :key="index" class="politician-main-createdCard-data">
          <div class="politician-main-createdCardImgContainer">
            <img class="politician-main-img" :src="russianScumbag.img" :alt="russianScumbag.name">
          </div>
          <div class="politician-main-createdCardInfoContainer">
            <h2>{{ russianScumbag.name }}</h2>
            <p> {{ russianScumbag.type }}</p>
            <p class="politician-main-card-info-networth">Rich Guy's Worth: {{ russianScumbag.riches ? `$${twoPlaceDeciChanger(russianScumbag.riches)}` : 'Man\'s a Brokie' }}</p> 
            <button class="politician-main-card-info-confiscateBtn" @click="toggleAssetsConfiscation(russianScumbag)">{{ russianScumbag.confiscated ? 'Return Confiscated Assets' : 'Confiscate Russian Assets' }}</button>
          </div>
        </div>
      </div>
    </div>
    <div class="main-page-politician-cart">
      <div class="main-page-politician-cart-bg">
        <h2 class="main-page-politician-cartTitle">Total Amount of Money Taken Away:</h2>
        <div class="main-page-politician-notConfiscated" v-if="confiscatedAssets.length === 0">No assets confiscated yet.</div>
        <div class="main-page-politician-Confiscated" v-else>
          <div v-for="(asset, index) in confiscatedAssets" :key="index">
            <p class="main-page-politician-ConfiscatedNames">{{ asset.name }} - ${{ twoPlaceDeciChanger(asset.riches) }}</p>
            <button class="main-page-politician-ConfiscatedMainReturnBtn" @click="returnConfiscatedAsset(index)">Return</button>
          </div>
          <p class="main-page-politician-ConfiscatedTotalAmnt">Total Confiscated Amount: {{ formattedConfiscatedAmount }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed } from 'vue';
import { ArrayOfRussianCorruptPoliticians } from '@/stores/wealthyRussians.js'; 

export default {
  setup() {
    const wealthyRussians = ref(ArrayOfRussianCorruptPoliticians);
    const confiscatedAssets = ref([]);
    const confiscatedAmount = ref(0);

    const toggleAssetsConfiscation = (russianScumbag) => {
      if (russianScumbag.riches > 0 && !russianScumbag.confiscated) { //If the value of assets is greater than 0 and assets have not been confiscated:
        russianScumbag.confiscated = true;
        confiscatedAmount.value += russianScumbag.riches;
        confiscatedAssets.value.push({ name: russianScumbag.name, riches: russianScumbag.riches });
        // Set the rich guy's worth to 0 when assets are confiscated
        russianScumbag.originalRiches = russianScumbag.riches;
        russianScumbag.riches = 0;
      } else if (russianScumbag.confiscated) {
        russianScumbag.confiscated = false;
        const index = confiscatedAssets.value.findIndex(asset => asset.name === russianScumbag.name);
        if (index !== -1) {
          confiscatedAmount.value -= confiscatedAssets.value[index].riches;
          confiscatedAssets.value.splice(index, 1);
          // Reset the rich guy's worth to the original amount when assets are returned
          russianScumbag.riches = russianScumbag.originalRiches;
        }
      }
    };

    const returnConfiscatedAsset = (index) => {
      confiscatedAmount.value -= confiscatedAssets.value[index].riches;
      const russianScumbag = wealthyRussians.value.find(russianScumbag => russianScumbag.name === confiscatedAssets.value[index].name);
      if (russianScumbag) {
        russianScumbag.confiscated = false;
        // Reset the rich guy's worth to the original amount when assets are returned
        russianScumbag.riches = russianScumbag.originalRiches;
      }
      confiscatedAssets.value.splice(index, 1);
    };

    const formattedConfiscatedAmount = computed(() => {
      const billions = confiscatedAmount.value / 1e9;
      return `$${billions.toFixed(2)} Billion`;
    });

    const twoPlaceDeciChanger = (value) => {
      return new Intl.NumberFormat('en-US', { 
        style: 'decimal', 
        minimumFractionDigits: 2, 
        maximumFractionDigits: 2 
      }).format(value);
    };

    return { wealthyRussians, confiscatedAssets, toggleAssetsConfiscation, returnConfiscatedAsset, formattedConfiscatedAmount, twoPlaceDeciChanger };
  }
};
</script>

<style scoped>
  .main-page-politician-ConfiscatedTotalAmnt{
    margin-top: 2rem;
    font-size: var(--h5);
  }
  .main-page-politician-ConfiscatedNames{
    font-size: var(--h4);
  }
  .main-page-politician-cart-bg{
    background-color: orange;
    padding: 1rem;
    width: 59rem;
    margin: 1.5rem;
  }
  .main-page-politician-cartTitle {
    padding: 2rem;
    font-family: 'Courier New', Courier, monospace;
    font-size: var(--h2);
  }
  .main-page-politician-notConfiscated {
    font-family: 'Courier New', Courier, monospace;
    font-size: var(--h3);
    margin-left: 2rem;
    margin-right: 2rem;
  }
  .main-page-politician-Confiscated{
    font-family: 'Courier New', Courier, monospace;
    font-size: var(--h4);
    margin-left: 2rem;
    margin-right: 2rem;
  }
  .main-page-politician-cart {
    background-color: beige;
    width: 61.6rem;
  }
  .politician-main-card-info-confiscateBtn {
    background-color: black;
    border-color: white;
    color: azure;
    width: 20rem;
    height: 3rem;
    border: 0rem;
    border-radius: .2rem;
    margin-top: 1rem;
  }
  .main-page-politician-ConfiscatedMainReturnBtn {
    background-color: black;
    border-color: white;
    color: azure;
    width: 15rem;
    text-align: calc(100% - 1rem);
    font-size: var(--h5);
    border-radius: .2rem;
    margin-top: 1rem;
  }
  .main-page {
    display: flex;
  }
  .politicians {
    width: 110rem;
  }
  .politicians-main-heading {
    background-color: beige;
    font-size: var(--h2);
    font-family: "Ojuju", sans-serif;
    font-optical-sizing: auto;
    font-style: bold;
    padding-top: 0rem;
    padding-left: 2rem;
    padding-right: 0rem;
    padding-bottom: 0.5rem;
  }
  .politician-main-createdCard {
    display: flex; 
    grid-column: 2;
    padding: 1rem;
    margin: 1rem;
    background-color: beige;
    justify-content: space-evenly;
    flex-wrap: wrap;
    border-radius: .5rem;
  }
  .politician-main-createdCard-data {
    border-radius: .5rem;
    background-color: orange;
    padding: 2rem;
    margin: 1rem;
  }
  .politician-main-img{
    border-radius: .1rem;
    height: 30rem;
    overflow: hidden;
    object-fit: cover;
    width: 28rem;
  }
  .politician-main-createdCardInfoContainer {
    margin-top: 1rem;
    font-size: var(--h4);
    font-family: "Roboto";
  }
  .politician-main-card-info-networth {
    font-style: italic;
    font-weight: bolder;
  }
</style>
