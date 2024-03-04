<template>
  <div class="ConfiscatedAssetsCard">
    <h1>Seized Property / Funds / Stocks</h1>
    <div v-if="confiscatedAssets.length === 0">
      Get to work, can't be letting these corrupt Russian businessmen and
      politicians take over the world! Too much time wasted don't you think?!
    </div>
    <div v-else>
      <div v-for="(item, index) in confiscatedAssets" :key="index">
        <p>{{ item.name }} - ${{ twoPlaceDeciChanger(item.riches) }}</p>
        <button @click="removeFromConfiscatedAssetsList(index)">Remove</button>
      </div>
      <p>Seized: {{ formattedConfiscatedAmount }}</p>
    </div>
  </div>
</template>

<script setup>
const confiscatedAssets = props.confiscatedAssets;
const formattedConfiscatedAmount = props.formattedConfiscatedAmount;
const twoPlaceDeciChanger = props.twoPlaceDeciChanger;

const removeFromConfiscatedAssetsList = (index) => {
  props.removeFromConfiscatedAssetsList(index);
};
</script>

<style scoped></style>
