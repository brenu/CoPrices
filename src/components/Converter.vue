<template>
  <div class="converter">
    <h2>{{firstCoin}} to {{secondCoin}}</h2>
    <input type="text" v-model="firstCoin_value" v-bind:placeholder="firstCoin" />
    <input type="button" value="Convert" v-on:click="convert" />
    <h2>{{secondCoin_value}}</h2>
  </div>
</template>

<script>
export default {
  name: "Converter",
  props: ["firstCoin", "secondCoin"],

  data() {
    return {
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
  padding: 20px;
  max-width: 300px;
  box-shadow: 0 4px 8px 0px rgba(0, 0, 0, 0.2);
}
</style>
