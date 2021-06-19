<template>
  <img alt="Bitcoin" src="./../assets/btc.png" />

  <h1>Bitcoin in Real Time</h1>
  <div>
    <h2>USD</h2>
    <div>${{ usd }}</div>
    <h2>BRL</h2>
    <div>R${{ brl }}</div>

    <h2>{{ time }}</h2>
  </div>
</template>

<script>
export default {
  name: "BtcPrice",
  data() {
    return {
      time: 30,
      brl: 0,
      usd: 0
    }
  },
  methods: {
    timer() {
      setInterval(() => {
        this.time--;

        if (this.time < 0) {
          this.time = 30;
          this.fetchPrices();
        }
      }, 1000);
    },

    async fetchPrices() {
      await fetch(
        `https://economia.awesomeapi.com.br/json/last/BTC-BRL,BTC-USD`
      )
        .then((response) => response.json())
        .then((data) => {
          this.brl = data["BTCBRL"]["high"];
          this.usd = data["BTCUSD"]["high"];
        })
        .catch((error) => console.log(`Error to fetch data: ${error}`));
    }
  },
  created() {
    this.fetchPrices();
  },
  beforeMount() {
    this.timer();
  },
}
</script>
