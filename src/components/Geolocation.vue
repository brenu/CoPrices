<template>
  <div class="geolocation" v-cloak>
    <div v-if="errorStr">Sorry, but the following error occurred: {{errorStr}}</div>

    <div v-if="gettingLocation">
      <span>Getting your location...</span>
    </div>
  </div>
</template>

<script>
export default {
  name: "Geolocation",
  data: () => ({
    location: null,
    latitude: null,
    logitude: null,
    gettingLocation: false,
    errorStr: null,
  }),
  created() {
    //do we support geolocation
    if (!("geolocation" in navigator)) {
      this.errorStr = "Geolocation is not available.";
      return;
    }

    this.gettingLocation = true;
    // get position
    navigator.geolocation.getCurrentPosition(
      (pos) => {
        this.gettingLocation = false;
        this.location = pos;
        this.latitude = location.coords.latitude;
        this.longitude = location.coords.longitude;
      },
      (err) => {
        this.gettingLocation = false;
        this.errorStr = err.message;
      }
    );
  },
};
</script>

<style scoped>
/** */
</style>