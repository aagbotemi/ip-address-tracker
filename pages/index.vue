<template>
  <div v-if="loading">Loading...</div>
  <div v-else>
    <header class="text-center pt-7 pb-52 sm:pb-7">
      <h1 class="text-white text-2xl sm:text-4xl">IP Address Tracker</h1>
      <form class="flex justify-center align-center mt-5 mb-7 sm:mb-10" @submit.prevent="getIP">
        <input
          v-model="search"
          class="shadow border rounded-l w-80 text-gray-700 sm:py-3 py-2 px-3 focus:outline-none focus:shadow-outline"
          type="text"
          name="search"
          placeholder="Search for any IP address or domain"
        />
        <button class="bg-white hover:bg-purple-600 rounded-r px-3" type="submit">
          <img src="/images/icon-arrow.svg">
        </button>
      </form>
      <IPTrackerCard :ip-address="ipAddress"/>
    </header>
    <div id="map-wrap" style="height: 100vh">
      <client-only>
        <l-map :zoom='zoom' :center="[latitude, longitude]">
          <l-tile-layer url="http://{s}.tile.osm.org/{z}/{x}/{y}.png"></l-tile-layer>
          <l-marker :lat-lng="[latitude, longitude]"></l-marker>
        </l-map>
      </client-only>
    </div>
  </div>
</template>

<script>
import 'leaflet/dist/leaflet.css'
import IPTrackerCard from "../components/IPTrackerCard";

export default {
  components: {
    IPTrackerCard,
  },
  data: () => ({
    loading: false,
    search: '',
    ipAddress: {},
    latitude: null,
    longitude: null,
    zoom: 13
  }),
  mounted() {
    this.getIP()
  },
  methods: {
    async getIP() {
      this.loading = true
      try {
        await this.$axios.get(this.$config.baseUrl + '?apiKey=' + this.$config.apiKey + `&ipAddress=${this.search}`)
          .then(res => {
            this.ipAddress = res.data
            this.latitude = this.ipAddress.location.lat
            this.longitude = this.ipAddress.location.lng
            this.search = ''
          });
      } catch (error) {
        alert(error.message)
      } finally {
        this.loading = false
      }
    },
  },
}
</script>

<style >
*, ::before, ::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

header {
  background-image: url('/images/pattern-bg.png');
  min-height: 15rem;
}
</style>
