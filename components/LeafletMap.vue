<script lang="ts" setup>
const map = ref(null);
const clickEvent = ref(null);

onMounted(() => {
  const L = window.L;
  map.value = new L.Map("map", {
    center: [13.123453245, 100.34564767567],
    zoom: 8,
    cursor: true,
    layers: [
      new L.TileLayer("http://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
        attribution:
          'Map data © <a href="http://openstreetmap.org">OpenStreetMap</a> contributors',
      }),
    ],
  });

  clickEvent.value = (e) => {
    getLatLon(e);
  };
  map.value.on("click", clickEvent.value);
});

watchEffect(() => {
  map.value;
});

const getLatLon = (e) => {
  if (e.originalEvent) {
    const lat = e.latlng.lat;
    const lon = e.latlng.lng;

    const popup = L.popup()
      .setLatLng([lat, lon])
      .setContent(`Latitude: ${lat}<br>Longitude: ${lon}`)
      .openOn(map.value);
  }
};
</script>

<template>
  <div ref="map" id="map" @click="getLatLon"></div>
</template>

<style scoped>
#map {
  height: 100vh;
  width: 100%;
  cursor: pointer;
}
</style>
