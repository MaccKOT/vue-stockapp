<template>
  <section>
    <!-- error message -->
    <div v-if="errors.length > 0" class="error">
      <p>We have errors: {{ JSON.stringify(errors, null, 2) }}</p>
    </div>

    <!-- cards -->
    <h2 v-if="loading">Loading data...</h2>
    <div v-else class="stock">
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

    <!-- info -->
    <h3>Get Info</h3>
    <select v-model="selected">
      <option value disabled>Select Company</option>
      <option
        v-for="value in stock"
        :key="value.stock"
        :value="value.description"
      >
        {{ value.companyName }}
      </option>
    </select>
    <div class="info">
      {{ selected }}
    </div>
  </section>
</template>

<script>
export default {
  name: 'Stocks',
  data() {
    return {
      stock: [],
      errors: [],
      selected: '',
      loading: true,
    };
  },
  created() {
    fetch(
      `https://financialmodelingprep.com/api/v3/profile/AAPL,NVDA,TSLA,AMD,INTC,MDB,SPCE,V,DAL,DOCU,OKTA,AMZN,PINS,TRIP,GDDY,DIS,MCD,NOK,UPWK,IBM,FB,ZM,OZON,NFLX,EA,HLT,H,CCL?apikey=${
        import.meta.env.SNOWPACK_PUBLIC_API_KEY
      }`
    )
      .then((responce) => responce.json())
      .then((data) => {
        this.stock = data;
        this.loading = false;
        // console.log(data)
      })
      .catch((e) => {
        this.errors.push(e);
      });
  },
  watch: {
    selected() {
      console.log(this.selected);
    },
  },
};
</script>

<style scoped>
.error {
  color: crimson;
}
</style>
