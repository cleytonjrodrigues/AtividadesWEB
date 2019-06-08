<template>

  <div class="row" id="app">
    <div class="col-6">
      <fieldset>
        <input type="text/javascript"
              ref="baseValue"
              value=""
              v-model="inputValue"
              @keyup="convertValue()">
        <b-dropdown v-bind:text="inputCurrency" class="m-2" variant="primary">
          <b-dropdown-item v-for="(name,value) in names" :key="value.value"
                          @click="inputCurrency = value; convertValue()">{{value}}</b-dropdown-item>
        </b-dropdown>
        Convert to
        <b-dropdown v-bind:text="outputCurrency" class="m-2" variant="success">
        <b-dropdown-item v-for="(name,value) in names" :key="value.value"
                        @click="outputCurrency = value; convertValue()">{{value}}</b-dropdown-item>
        </b-dropdown>
        <input type="text/javascript"
            ref="baseValue"
            value=""
            v-model="outputValue"
            @keyup="convertValue()">
      </fieldset>

    </div>
  </div>

</template>

<script>
export default {
  name: 'app',
  data() {
    return {
      currencies: [],
      names: [],
      currentBase: [],
      inputCurrency: "--",
      outputCurrency: "--",
      inputValue: null,
      outputValue: null
    }
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
  },
  methods: {
    convertValue() {
      let url = 'https://api.exchangeratesapi.io/latest?base=' + this.inputCurrency + '&symbols=' + this.outputCurrency;
      let self = this;
      this.$axios.get(url)
      .then( function(response) {
        self.currentBase = response.data.rates;
        console.log(self.currentBase[0]);

        for(let i in self.currentBase) {
          self.outputValue = self.currentBase[i]*self.inputValue;
        }
      })
    }
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