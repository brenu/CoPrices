<template>
  <div class="geolocation" v-cloak>
    <div v-if="errorStr">Sorry, but the following error occurred: {{ errorStr }}</div>
    <div v-if="regionCoin">
      <p class="geolocation-text">
        According to our location methods, the coin of your country is
        {{ regionCoin }}. Is it right?
      </p>
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
    country: null,
    gettingLocation: false,
    errorStr: null,
    regionCoin: null,
  }),
  methods: {
    getLocation() {
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
                this.country = results[0].Result[0].Location.Address.Country;
                return this.getCoin();
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
    getCoin() {
      const url = "https://restcountries.eu/rest/v2/alpha/" + this.country;

      fetch(url)
        .then((res) => {
          return res.json();
        })
        .then((json) => {
          this.regionCoin = json.currencies[0].code;
          this.submit();
          return this.regionCoin;
        });
    },
    submit() {
      this.$emit("region-update", this.regionCoin);
    },
  },
  async created() {
    this.getLocation();
  },
};
</script>

<style scoped>
.geolocation {
  margin-top: 20px;
}
.geolocation-text {
  width: 340px;
  color: #fff;
  align-self: center;
  text-align: justify;
  font-size: 16pt;
}

@media (min-width: 700px) {
  .geolocation-text {
    width: 399px;
    text-align: center;
  }
}
</style>
