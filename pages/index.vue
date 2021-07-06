<template>
  <div>
    <header >
      <h1>IP Address Tracker</h1>
      <form @submit.prevent="getIP">
        <input type="text"
               name="search"
               placeholder="Search for any IP address or domain"
               v-model="search"
        />
        <button type="submit"><img src="/images/icon-arrow.svg"></button>
      </form>

      <section class="details">
        <article>
          <div class="ip-address">
            <h3>IP ADDRESS</h3>
            <span>192.786.896.9</span>
          </div>
          <div class="location">
            <h3>LOCATION</h3>
            <span>
              Ikeja, Lagos Nigeria
            </span>
          </div>
          <div class="timezone">
            <h3>TIMEZONE</h3>
            <span>GMT +1:00</span>
          </div>
          <div class="isp">
            <h3>ISP</h3>
            <span>MTN</span>
          </div>
        </article>
      </section>
    </header>
  </div>
</template>

<script>
export default {
  data: () => ({
    search: ''
  }),
  methods: {
    getIP() {
      //
    }
  }
}
</script>
