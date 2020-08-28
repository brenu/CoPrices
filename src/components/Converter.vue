<template>
  <div class="converter-container">
    <div class="drawer">
      <div class="converter">
        <div class="selects-container">
          <select v-model="firstCoin">
            <option v-for="coin in coins" :key="coin">{{ coin }}</option>
          </select>
          <span>to</span>
          <select v-model="secondCoin" v-on:region-update="handleRegionUpdate">
            <option v-for="coin in coins" :key="coin">{{ coin }}</option>
          </select>
        </div>
        <div class="input-container">
          <input
            type="text"
            v-model="firstCoin_value"
            v-bind:placeholder="firstCoin"
          />
          <button type="button" v-on:click="convert">Convert</button>
        </div>
      </div>
      <h2>{{ secondCoin }} {{ secondCoin_value }}</h2>
    </div>

    <Geolocation @region-update="handleRegionUpdate" />
  </div>
</template>

<script>
import Geolocation from "../components/Geolocation";

export default {
  name: "Converter",
  data() {
    return {
      coins: [
        "CAD",
        "HKD",
        "ISK",
        "PHP",
        "EUR",
        "DKK",
        "HUF",
        "CZK",
        "AUD",
        "RON",
        "SEK",
        "IDR",
        "INR",
        "BRL",
        "RUB",
        "HRK",
        "JPY",
        "THB",
        "CHF",
        "SGD",
        "PLN",
        "BGN",
        "TRY",
        "CNY",
        "NOK",
        "NZD",
        "ZAR",
        "USD",
        "MXN",
        "ILS",
        "GBP",
        "KRW",
        "MYR",
      ],
      firstCoin: "USD",
      secondCoin: "EUR",
      firstCoin_value: "1.00",
      secondCoin_value: 0.0,
    };
  },
  components: {
    Geolocation,
  },
  methods: {
    convert() {
      if (this.firstCoin_value) {
        const url =
          "https://api.exchangeratesapi.io/latest?base=" +
          this.firstCoin +
          "&symbols=" +
          this.secondCoin;

        fetch(url)
          .then((res) => {
            return res.json();
          })
          .then((json) => {
            const cotation = json["rates"][this.secondCoin];
            this.secondCoin_value = (
              cotation * parseFloat(this.firstCoin_value)
            ).toFixed(2);
          });
      }
    },
    handleRegionUpdate($event) {
      this.secondCoin = $event;
      this.convert();
    },
  },
  created() {
    this.convert();
  },
};
</script>

<style scoped>
.converter-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.drawer {
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 5px;
  box-shadow: 0 4px 8px 0px rgba(0, 0, 0, 0.2);
}

.converter {
  display: flex;
  flex-direction: column;
  justify-content: ceter;
  align-items: center;
  max-width: 300px;
  background-color: #fff;
  border-radius: 5px;
  padding: 20px;
  box-shadow: 0 4px 8px 0px rgba(0, 0, 0, 0.2);
}

.input-container {
  margin-top: 20px;
  width: 100%;
  display: flex;
  padding-left: 50px;
  padding-right: 50px;
  margin-bottom: 10px;
  justify-content: center;
}

select {
  font-size: 20pt;
  font-weight: bold;
  background-color: #fff;
  color: #4bb543;
  border: 0;
  padding-bottom: 0px;
  text-indent: 3px;
  box-shadow: 0px 0px 3px 0px rgba(0, 0, 0, 0.2);
  border-radius: 3px;
  -moz-appearance: none;
  -webkit-appearance: none;

  cursor: pointer;
}

select option {
  align-self: baseline;
}

select::-ms-expand {
  display: none;
}

span {
  font-size: 18pt;
  color: #999;
  font-weight: normal;
  margin: 10px;
}

input {
  width: 100px;
  background-color: #fff;
  border: 0;
  border-bottom: 1px solid #4bb543;
  padding: 10px;
  margin-right: 20px;
}

button {
  background-color: #4bb543;
  border: 0;
  border-radius: 4px;
  padding: 10px;

  font-size: 12pt;
  color: #fff;

  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #3a9133;
}
</style>
