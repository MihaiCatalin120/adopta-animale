<template>
  <div class="h-full map">
    <MglMap :map-style="style" :center="center" :zoom="zoom">
      <MglMarker v-for="(feature, index) in data.features" :key="index" :coordinates="feature.geometry.coordinates">
        <template #marker>
          <div style="scale: 1.5">
            {{ shelterIconMapping[feature.properties.symbol] }}
          </div>
        </template>

        <MglPopup ref="popup" :close-button="false" @open="onPopupOpen">
          <div id="popup">
            <Icon name="material-symbols:close" class="absolute top-1 right-1 cursor-pointer" @click="onPopupClose" />
            <h1>{{ feature.properties.name }}</h1>
            <p>{{ feature.properties.address }}</p>
            <p>{{ feature.properties.schedule }}</p>
          </div>
        </MglPopup>
      </MglMarker>
      <MglNavigationControl />
    </MglMap>
  </div>
</template>

<script setup>
import { gsap } from 'gsap';
import data from '~/app/assets/data/geo/all-shelters-data.json';
const style = `https://api.maptiler.com/maps/streets-v2/style.json?key=jUbqVn99qizW6y4sYQZS`;
const center = [25.0094303, 45.9442858];
const zoom = 6;

const shelterIconMapping = {
  cat: '🐱',
  dog: '🐶'
}

const onPopupOpen = () => {
  gsap.fromTo('.maplibregl-popup', { opacity: 0, scale: 0, transformOrigin: 'bottom center' }, { opacity: 1, scale: 1, transformOrigin: 'bottom center', duration: 0.2 });
}

const onPopupClose = () => {
  gsap.fromTo('.maplibregl-popup', { opacity: 1, scale: 1, transformOrigin: 'bottom center' }, { opacity: 0, scale: 0, transformOrigin: 'bottom center', duration: 0.2 });
}

</script>

<style>
.map {
  .maplibregl-popup-content {
    color: black;
  }

  .maplibregl-popup-close-button {
    padding: 4px;
  }
}
</style>
