<template>
  <div class="container">
    <div class="row">
      <h1 class="pt-4">Coin Market</h1>
      <input
        type="text"
        class="form-control text-light bg-dark rounded-0 border-0 my-4"
        placeholder="Search"
        v-model="textSearch"
        @keyup="searchCoin()"
        autofocus
      />
      <!-- Si se da un error al hacer un get a la api, salta este mensaje en rojo -->
      <section v-if="errored">
        <h4 class=" h4 text-center text-danger">
          Lo sentimos, no es posible obtener la información en este momento, por
          favor intente nuevamente mas tarde
        </h4>
      </section>

    <!-- Aparece cuando mientras no se cargue toda la informacion pedida -->
      <section v-else>
        <div v-if="loading">Cargando...</div>

        <table class="table table-dark">
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
                {{ coin.price_change_percentage_24h }} %
              </td>
              <td>$ {{ coin.total_volume.toLocaleString() }}</td>
            </tr>
          </tbody>
        </table>
      </section>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  //data es un metodo que retorna un objeto
  data() {
    return {
      coins: [], //arreglo donde van a ir los datos de la api
      filteredCoins: [],
      loading: true,
      errored: false,
      titles: ["#", "Coin", "Price", "Price Change", "24h Volume"],
      textSearch: "",
    };
  },
  // en axios los datos estan en data, es por eso que el resultado se obtiene así
  async mounted() {
    await axios
      .get(
        "https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false"
      )
      .then((response) => {
        this.coins = response.data;
        this.filteredCoins = response.data;
      })
      .catch((error) => {
        console.log(error);
        this.errored = true;
      })
      .finally(() => (this.loading = false));
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


