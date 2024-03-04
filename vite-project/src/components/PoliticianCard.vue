<template>
  <div class="PoliticianData">
    <div class="CreatedPoliticianCardImgContainer">
      <img class="PoliticianImg" :src="politician.img" :alt="politician.name" />
    </div>
    <div class="CreatedPoliticianCardInfoContainer">
      <h2>{{ politician.name }}</h2>
      <p>{{ politician.type }}</p>
      <p>
        Rich Guy's Worth:
        {{
          politician.riches
            ? `$${twoPlaceDeciChanger(politician.riches)}`
            : "Man's a Brokie"
        }}
      </p>
      <button @click="confiscateOrReturnAssets">
        {{
          politician.confiscated
            ? "Return Confiscated Assets"
            : "Confiscate Russian Assets"
        }}
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { Howl } from "howler";
const politician = props.politician;
const twoPlaceDeciChanger = props.twoPlaceDeciChanger;
const playConfiscateSound = () => {
  const sound = new Howl({
    src: "vite-project/public/mp3files/boowomp.mp3",
  });
  sound.play();
};
const confiscateOrReturnAssets = () => {
  if (politician.riches > 0 && !politician.confiscated) {
    props.confiscateAssets(politician);
    playConfiscateSound();
  } else {
    props.returnAssets(politician);
  }
};
</script>

<style scoped></style>
