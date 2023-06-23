<script setup lang="ts">
import {LIcon, LMap, LMarker, LPopup, LTileLayer} from "@vue-leaflet/vue-leaflet";
import {onMounted, ref} from "vue";

const zoom = ref(14)
const map = ref(null)
const myLocation = ref({lat: 0, lon: 0})
const loaded = ref(false)

const alerti = () => {
  window.alert('Yeah')
}

const options = {
  enableHighAccuracy: true,
  timeout: 5000,
  maximumAge: 0,
};

const success = (pos: { coords: Record<any, any> }) => {
  const crd = pos.coords;

  myLocation.value = {
    lat: crd.latitude,
    lon: crd.longitude,
  }
  loaded.value = true
}

const error = (err: any) => {
  console.warn(`ERROR(${err.code}): ${err.message}`);
}

onMounted(() => {
  navigator.geolocation.getCurrentPosition(success, error, options);
})
</script>
<template>
  <div style="height:100vh; width:100vw">
    <l-map v-if="loaded" ref="map" v-model:zoom="zoom" :center="[myLocation.lat, myLocation.lon]" :use-global-leaflet="false">
      <l-tile-layer
          url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"
          layer-type="base"
          name="OpenStreetMap"
      ></l-tile-layer>
      <l-marker :lat-lng="[47.8102105,13.0266327]">
        <l-popup>
          <h3>Badminton</h3>
          <div> Sed posuere consectetur est at lobortis. Aenean eu leo quam. Pellentesque ornare
            sem lacinia quam venenatis vestibulum.
          </div>
        </l-popup>
      </l-marker>
      <l-marker :draggable="true" @dragend="alerti()"  :lat-lng="[47.8102105,13.0166327]"/>
      <l-marker :lat-lng="[myLocation.lat, myLocation.lon]">
        <l-icon icon-url="https://raw.githubusercontent.com/pointhi/leaflet-color-markers/master/img/marker-icon-2x-red.png" :icon-size="[25,41]" />
      </l-marker>
    </l-map>
  </div>
</template>
<style>
body {
  margin: 0;
  padding: 0;
}
</style>