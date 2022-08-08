<template>
  <h1>hello</h1>
</template>

<script>

export default {
  name: 'App',
  //cuando cargue la app va a hacer esta petición fetch a la api
  async mounted(){
    const resp = await fetch('https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false')
    const data = await resp.json() //convierte en formato json
    console.log(data)
  }
}
</script>

<style>

</style>
