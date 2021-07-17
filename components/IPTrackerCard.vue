<template>
  <section class="z-10 w-full flex justify-center absolute">
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
</template>

<script>
export default {
  name: "IPTrackerCard",
  props: {
    ipAddress: {
      type: Object,
      required: true
    }
  }
}
</script>

<style scoped>
.z-10 {
  z-index: 1000;
}

</style>
