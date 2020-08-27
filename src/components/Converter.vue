<template>
  <div class="converter">
    <select v-model="firstCoin">
      <option v-for="coin in coins" :key="coin">{{coin}}</option>
    </select>
    to
    <select v-model="secondCoin">
      <option v-for="coin in coins" :key="coin">{{coin}}</option>
    </select>
    <input type="text" v-model="firstCoin_value" v-bind:placeholder="firstCoin" />
    <input type="button" value="Convert" v-on:click="convert" />
    <h2>{{secondCoin}} {{secondCoin_value}}</h2>
  </div>
</template>

<script>
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
      secondCoin: "BRL",
      firstCoin_value: "",
      secondCoin_value: 0,
    };
  },

  methods: {
    convert() {
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
    },
  },
};
</script>

<style scoped>
.converter {
  max-width: 300px;
  background-color: #fff;
  border-radius: 5px;
  padding: 20px;
  box-shadow: 0 4px 8px 0px rgba(0, 0, 0, 0.2);
}
</style>
