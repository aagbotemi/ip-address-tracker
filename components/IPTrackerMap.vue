<template>
  <div id="map-wrap" style="height: 100vh">
    <client-only>
      <l-map :zoom='zoom' :center="[latitude, longitude]">
        <l-tile-layer url="http://{s}.tile.osm.org/{z}/{x}/{y}.png"></l-tile-layer>
        <l-marker :lat-lng="[latitude, longitude]"></l-marker>
      </l-map>
    </client-only>
  </div>
</template>

<script>
export default {
  name: "IPTrackerMap",
  props: {
    latitude: {
      type: null,
      required: true
    },
    longitude: {
      type: null,
      required: true
    },
    zoom: {
      type: Number,
      required: true
    }
  }
}
</script>

<style scoped>

</style>
