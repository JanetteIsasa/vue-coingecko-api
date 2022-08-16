<template>
  <div class="container">
    <div class="row">
      <h1 class="pt-4">Coin Market</h1>
      <div class="align-items-center">
        <input
          type="text"
          class="form-control text-light bg-dark rounded-0 border-0 my-4"
          placeholder="Search"
          v-model="textSearch"
          @keyup="searchCoin()"
          autofocus
        />
      </div>

<div class="table-responsive">
   <table class="table table-dark table-hover"  style="width: 100%">
        <thead>
          <tr>
            <th v-for="title in titles" :key="title">
              {{ title }}
            </th>
          </tr>
        </thead>
        <tbody v-for="(coin, index) in filteredCoins" :key="coin.id">
          <tr>
            <td class="text-muted">
              {{ index + 1 }}
            </td>
            <td>
              <img :src="coin.image" style="width: 2rem" class="me-2" />
              <span>
                {{ coin.name }}
              </span>
              <span class="ms-2 text-uppercase text-muted">
                {{ coin.symbol }}
              </span>
            </td>
            <td>${{ coin.current_price }}</td>
            <td
              :class="[
                coin.price_change_percentage_24h > 0
                  ? 'text-success'
                  : 'text-danger',
              ]"
            >
              {{ coin.price_change_percentage_24h }}%
            </td>
            <td>$ {{ coin.total_volume.toLocaleString() }}</td>
          </tr>
        </tbody>
      </table>
</div>
     
      
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  //data es un metodo que retorna un objeto
  data() {
    return {
      coins: [], //arreglo donde van a ir los datos de la api
      filteredCoins: [],
      titles: ["#", "Coin", "Price", "Price Change", "24h Volume"],
      textSearch: "",
    };
  },
  //cuando cargue la app va a hacer esta petición fetch a la api
  async mounted() {
    const resp = await fetch(
      "https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false"
    );
    const data = await resp.json(); //convierte en formato json
    this.coins = data; //pasa los datos al objeto coins
    this.filteredCoins = data;
  },
  //evento creado para filtrar a la par que el usuario tipea
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


