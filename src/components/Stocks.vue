<template>
  <div class="container">
    <div class="stock">
      <div class="stock-item" v-for="value in stock" :key="value.stock">
        <div class="stock-item__info">
          <div class="stock-item__cover">
            <img :src="value.image" :alt="value.companyName" />
          </div>
          <h3 class="stock-item__title">
            {{ value.companyName }}
            <span>{{ value.symbol }}</span>
          </h3>
        </div>
        <span class="stock-item__price">{{ value.price }} $</span>
      </div>
    </div>
    <h3>Get Info</h3>
    <select v-model="selected">
      <option value disabled>Select Company</option>
      <option
        v-for="value in stock"
        :key="value.stock"
        :value="value.description"
      >{{ value.companyName }}</option>
    </select>
    <div class="info">{{ selected }}</div>
  </div>
</template>

<script>
export default {
  name: 'Stocks',
  data()
  {
    return {
      stock: [],
      errors: [],
      selected: ''
    }
  },
  created()
  {
    fetch(`https://financialmodelingprep.com/api/v3/profile/AAPL,NVDA,TSLA,AMD,INTC,MDB,SPCE,V,DAL,DOCU,OKTA,AMZN,PINS,TRIP,GDDY,DIS,MCD,NOK,UPWK,IBM,FB,ZM,OZON,NFLX,EA,HLT,H,CCL?apikey=${import.meta.env.SNOWPACK_PUBLIC_API_KEY}`)
      .then(responce => responce.json())
      .then(
        data =>
        {
          this.stock = data
          // console.log(data)
        }
      )
      .catch(e =>
      {
        this.errors.push(e)
      })
  }
}
</script>