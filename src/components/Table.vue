<template>
  <h1 class="my-3 p-0">💰 Coin Market 💰</h1>

  <div class="px-5">
    <input
      type="text"
      class="form-control w-full bg-dark text-light rounded-0 border-0 rounded-2 my-4"
      placeholder="Search Coin"
      @keyup="searchCoin()"
      v-model="textSearch"
    />
  </div>

  <div class="px-3">
    <div class="table-responsive-sm">
      <table class="table table-dark">
        <thead>
          <tr>
            <th v-for="title in titles" :key="title">{{ title }}</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(coin, i) in filteredCoins" :key="coin.id">
            <td>{{ i + 1 }}</td>
            <td>
              <img
                :src="coin.image"
                :alt="coin.name"
                class="me-2"
                style="width: 2rem"
              />
              {{ coin.name }}
              <span class="ms-2 text-uppercase text-muted">{{
                coin.symbol
              }}</span>
            </td>
            <td>${{ coin.current_price }}</td>
            <td
              :class="[
                coin.price_change_percentage_24h > 0
                  ? 'text-success'
                  : 'text-danger',
              ]"
            >
              {{ coin.price_change_percentage_24h }} %
            </td>
            <td>$ {{ coin.total_volume.toLocaleString() }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>


<script>
import { DummyData } from "../data.json";

export default {
  name: "TableComponent",
  data() {
    return {
      coins: [],
      filteredCoins: [],
      titles: ["#", "Coin", "Price", "Price Change", "24h Volumne"],
      textSearch: "",
    };
  },
  async mounted() {
    const res = await fetch(
      "https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false"
    );
    const data = await res.json();

    this.coins = data;
    this.filteredCoins = data;
  },
  methods: {
    searchCoin() {
      this.filteredCoins = this.coins.filter(
        (coin) =>
          coin.name.toLowerCase().includes(this.textSearch.toLowerCase()) ||
          coin.symbol.toLowerCase().includes(this.textSearch.toLowerCase())
      );
    },
  },
};
</script>

<style>
@media (max-width: 576px) {
  h1 {
    letter-spacing: 2px;
    text-align: center;
  }
}
</style>