<template>
  <div v-if="loading">Loading...</div>
  <div v-else>
    <header >
      <h1>IP Address Tracker</h1>
      {{ latitude }}
      {{ longitude }}
      <form @submit.prevent="getIP">
        <input
          v-model="search"
          type="text"
          name="search"
          placeholder="Search for any IP address or domain"
        />
        <button type="submit"><img src="/images/icon-arrow.svg"></button>
      </form>

      <section class="details">
        <article>
          <div class="ip-address">
            <h3>IP ADDRESS</h3>
            <span>{{ ipAddress.ip }}</span>
          </div>
          <div class="location">
            <h3>LOCATION</h3>
            <span>
              {{ ipAddress.location && ipAddress.location.city }},
              {{ ipAddress.location && ipAddress.location.region }},
              {{ ipAddress.location && ipAddress.location.country }}
            </span>
          </div>
          <div class="timezone">
            <h3>TIMEZONE</h3>
            <span>GMT {{ ipAddress.location && ipAddress.location.timezone }}</span>
          </div>
          <div class="isp">
            <h3>ISP</h3>
            <span>{{ ipAddress.isp }}</span>
          </div>
        </article>
      </section>
    </header>
  </div>
</template>

<script>
// import axios from '@nuxtjs/axios'

export default {
  /* async asyncData({ $axios, $config }) {
    const post = await $axios.$get($config.baseUrl + '?apiKey=' + $config.apiKey + `&ipAddress=${search}`);

    console.log(post)
    return { post }
  }, */
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
            console.log(res.data)
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


      // this.loading = false
    }
  },
}
</script>
