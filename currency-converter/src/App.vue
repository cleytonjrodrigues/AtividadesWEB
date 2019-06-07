<template>

  <div class="row" id="app">
    <div class="col-6">
      <fieldset>
      <b-dropdown id="dropdown-1">
        <b-dropdown-item v-for="(name,value) in names" :key="value.value">{{value}}</b-dropdown-item>
      </b-dropdown>
      <input type="text/javascript"
             ref="baseCurrency"
             value="">
      Convert to
    </fieldset>
    </div>
  </div>

</template>

<script>
export default {
  name: 'app',
  components: {
  },
  data() {
    return {
      currencies: [],
      names: []
    }
  },
  getRates(base) {
    let rates
    for (let key in base) {
      if (base.hasOwnProperty(key) && key == 'rates') {
        rates = self.currencies[key];
        console.log(rates)
      }
    }
    return rates
  },
  created() {
    console.log("ENTROU")

    var self = this
    let url = 'https://api.exchangeratesapi.io/latest'
    this.$axios.get(url)
    .then( function(response) {
        self.currencies = response.data
        for (let key in self.currencies) {
          if (self.currencies.hasOwnProperty(key) && key == 'rates') {
            self.names = self.currencies[key];
            console.log(self.names)
          }
        }
    })
  
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
}