<template>
  <div class="geolocation" v-cloak>
    <div v-if="errorStr">Sorry, but the following error occurred: {{errorStr}}</div>

    <div v-if="gettingLocation">
      <span>Getting your location...</span>
    </div>

    <div v-if="location">Your location data is {{location.Address.Label}}</div>
  </div>
</template>

<script>
export default {
  name: "Geolocation",
  data: () => ({
    location: null,
    latitude: null,
    logitude: null,
    country: null,
    gettingLocation: false,
    errorStr: null,
  }),
  async created() {
    //do we support geolocation
    if (!("geolocation" in navigator)) {
      this.errorStr = "Geolocation is not available.";
      return;
    }

    this.gettingLocation = true;

    const platform = new window.H.service.Platform({
      apikey: "hfadb5v_Tdwt5dSvrmg-0zIO3z_IUybkFnc3z_w0dkY",
    });

    const geocoder = platform.getGeocodingService();

    navigator.geolocation.getCurrentPosition(
      (pos) => {
        let reverseGeocodingParameters = {
          prox: `${pos.coords.latitude},${pos.coords.longitude}`,
          mode: "retrieveAddresses",
          maxresults: 1,
        };

        geocoder.reverseGeocode(
          reverseGeocodingParameters,
          (res) => {
            let results = res.Response.View;
            if (results.length === 0) {
              window.resolve("No match.");
            } else {
              this.gettingLocation = false;
              this.location = results[0].Result[0].Location;
              console.log(results);
              this.country = results[0].Result[0].Location.Address.Country;
              console.log(this.country);
            }
          },
          (e) => window.reject(e)
        );
      },
      (err) => {
        window.reject(err);
      }
    );
  },
};
</script>

<style scoped>
/** */
</style>