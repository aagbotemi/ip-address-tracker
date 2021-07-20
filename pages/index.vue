<template>
  <div v-if="loading">Loading...</div>
  <div v-else>
<!--    <client-only>
      <Particles
        color="#DCBA8F"
        :particles-number="100"
        shape-type="star"
        :particle-size="3"
        movement-direction="top"
        lines-color="#dedede"
        :line-linked="false"
        :move-speed="0.75"
      />
    </client-only>-->

    <header style="position:relative;" class="text-center pb-52 sm:pb-7">
      <no-ssr>
        <Particles
          id="tsparticles"
          :options="{
                    background: {
                        color: {
                            value: '#0d47a1'
                        }
                    },
                    fpsLimit: 60,
                    interactivity: {
                        detectsOn: 'canvas',
                        events: {
                            onClick: {
                                enable: true,
                                mode: 'push'
                            },
                            onHover: {
                                enable: true,
                                mode: 'repulse'
                            },
                            resize: true
                        },
                        modes: {
                            bubble: {
                                distance: 400,
                                duration: 10,
                                opacity: 0.8,
                                size: 40
                            },
                            push: {
                                quantity: 10
                            },
                            repulse: {
                                distance: 200,
                                duration: 4
                            }
                        }
                    },
                    particles: {
                        color: {
                            value: '#ffffff'
                        },
                        links: {
                            color: '#ffffff',
                            distance: 150,
                            enable: true,
                            opacity: 0.5,
                            width: 1
                        },
                        collisions: {
                            enable: true
                        },
                        move: {
                            direction: 'none',
                            enable: true,
                            outMode: 'bounce',
                            random: false,
                            speed: 5,
                            straight: false
                        },
                        number: {
                            density: {
                                enable: true,
                                value_area: 800
                            },
                            value: 150
                        },
                        opacity: {
                            value: 0.5
                        },
                        shape: {
                            type: 'circle'
                        },
                        size: {
                            random: true,
                            value: 5
                        }
                    },
                    detectRetina: false
                }"
        />
      </no-ssr>
      <div style="transform: translateY(-13rem)">
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
      </div>

    </header>
    <IPTrackerMap :zoom="zoom" :latitude="latitude" :longitude="longitude" />
  </div>
</template>

<script>
import 'leaflet/dist/leaflet.css'
import IPTrackerCard from "../components/IPTrackerCard";
import IPTrackerMap from "../components/IPTrackerMap";

export default {
  components: {
    IPTrackerMap,
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

/*header {
  background-image: url('/images/pattern-bg.png');
  min-height: 15rem;
}*/

.tsparticles {
  position: fixed;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  z-index: -1;
  background: #45d;
}
</style>
