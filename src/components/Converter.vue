<template>
  <div class="converter">
    <select v-model="firstCoin">
      <option v-for="coin in coins" :key="coin">{{coin}}</option>
    </select>
    <span>to</span>
    <select v-model="secondCoin">
      <option v-for="coin in coins" :key="coin">{{coin}}</option>
    </select>
    <input type="text" v-model="firstCoin_value" v-bind:placeholder="firstCoin" />
    <button type="button" v-on:click="convert">Convert</button>
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
      secondCoin_value: 0.0,
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

select {
  font-size: 20pt;
  font-weight: bold;
  background-color: #fff;
  color: #4bb543;
  border: 0;
  text-indent: 3px;
  -moz-appearance: none;
  -webkit-appearance: none;
}

select::-ms-expand {
  display: none;
}

span {
  font-size: 18pt;
  font-weight: lighter;
}

input {
  background-color: #fff;
  border: 0;
  border-bottom: 1px solid #4bb543;
  padding: 5px;
  margin-right: 5px;
}

button {
  background-color: #4bb543;
  border: 0;
  border-radius: 4px;
  padding: 6px;

  font-size: 12pt;
  color: #fff;

  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #3a9133;
}
</style>
