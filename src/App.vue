<template>
  <h1>hello</h1>
  <div class="container">
    <div class="row">
      <table class="table table-dark">
        <thead>
          <tr>
            <th v-for="title in titles" :key="title">
            {{title}}
            </th>
          </tr>
        </thead>
        <tbody v-for="(coin, index ) in coins" :key="coin.id">
          <tr>
            <td class="text-muted">
              {{index + 1}}
            </td>
            <td>
              <img :src="coin.image" style="width: 2rem;" class="me-2"/>
              <span>
                {{ coin.name }}
              </span>
              <span class="ms-2 text-uppercase text-muted">
                {{coin.symbol}}
              </span>
            </td>
            <td>
              ${{coin.current_price}}
            </td>
            <td>
              {{coin.price_change_percentage_24h}}
            </td>
            <td>
              {{coin.total_volume}}
            </td>
          </tr>
        </tbody>
      </table>
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
      titles: [
        '#',
        'Coin',
        'Price',
        'Price Change',
        '24h Volume',
      ]
    };
  },
  //cuando cargue la app va a hacer esta petición fetch a la api
  async mounted() {
    const resp = await fetch(
      "https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false"
    );
    const data = await resp.json(); //convierte en formato json
    this.coins = data; //pasa los datos al objeto coins
  },
};
</script>

<style>
/* tr td img-coin{
  width: 2 rem;
} */
</style>
