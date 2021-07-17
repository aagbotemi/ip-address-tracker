<template>
  <div v-if="loading">Loading...</div>
  <div v-else>
    <header class="text-center py-7">
      <h1 class="text-white text-2xl sm:text-4xl">IP Address Tracker</h1>
      <form class="flex justify-center align-center mt-5 mb-7 sm:mb-10" @submit.prevent="getIP">
        <input
          class="shadow border rounded-l w-80 text-gray-700 sm:py-3 py-2 px-3 focus:outline-none focus:shadow-outline"
          v-model="search"
          type="text"
          name="search"
          placeholder="Search for any IP address or domain"
        />
        <button class="bg-white hover:bg-purple-600 rounded-r px-3" type="submit">
          <img src="/images/icon-arrow.svg">
        </button>
      </form>
      <section class=" w-full flex justify-center absolute">
        <article class="sm:flex bg-white shadow-md rounded-md py-5 px-16 sm:p-5 mx-5">
          <div class="border-b sm:border-b-0 sm:border-r border-grey-900 sm:pr-3 pb-2 sm:pb-0">
            <h3 class="text-xs font-bold text-gray-500">IP ADDRESS</h3>
            <span class="font-medium text-lg sm:text-xl">{{ ipAddress.ip }}</span>
          </div>
          <div class="border-b sm:border-b-0 sm:border-r border-grey-900 sm:px-3 py-2 sm:py-0">
            <h3 class="text-xs font-bold text-gray-500">LOCATION</h3>
            <span class="font-medium text-lg sm:text-xl">
              {{ ipAddress.location && ipAddress.location.city }},
              {{ ipAddress.location && ipAddress.location.region }},
              {{ ipAddress.location && ipAddress.location.country }}
            </span>
          </div>
          <div class="border-b sm:border-b-0 sm:border-r border-grey-900 sm:px-3 py-2 sm:py-0">
            <h3 class="text-xs font-bold text-gray-500">TIMEZONE</h3>
            <span class="font-medium text-lg sm:text-xl">GMT {{ ipAddress.location && ipAddress.location.timezone }}</span>
          </div>
          <div class=" sm:pl-3 pt-2 sm:pt-0">
            <h3 class="text-xs font-bold text-gray-500">ISP</h3>
            <span class="font-medium text-lg sm:text-xl">{{ ipAddress.isp }}</span>
          </div>
        </article>
      </section>
    </header>
  </div>
</template>

<script>
// import axios from '@nuxtjs/axios'

export default {
  data: () => ({
    loading: false,
    search: '',
    ipAddress: {},
    latitude: null,
    longitude: null,
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
    }
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
