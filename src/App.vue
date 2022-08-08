<template>
  <h1>hello</h1>
  <table>
    <thead>
      <tr>
        <th>coin</th>
      </tr>
    </thead>
    <tbody v-for="coin in coins" :key="coin.id">
      <tr>
        {{coin.name}}
      </tr>
    </tbody>
  </table>
</template>

<script>

export default {
  name: 'App',
  //data es un metodo que retorna un objeto
  data(){
    return{
      coins:[],
    }
  },
  //cuando cargue la app va a hacer esta petición fetch a la api
  async mounted(){
    const resp = await fetch('https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false');
    const data = await resp.json(); //convierte en formato json
    this.coins = data;
  }
}
</script>

<style>

</style>
