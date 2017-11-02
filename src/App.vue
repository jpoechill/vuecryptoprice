<template>
  <div id="app">
    <div class="crypto-container max-height">
      <b-row class="max-height">
        <b-col xs="12" sm="6" class="column-left">
          <div class="currency-container">
            Bitcoin<span class="heart-icon">❤️</span><br/>
            Ethereum<br/>
            Litecoin<br/>
          </div>
        </b-col>
        <b-col xs="12" sm="6" class="column-right">
          <div class="price-container">
            ${{BTCPrice}}<br/>
            ${{ETHPrice}}<br/>
            ${{LTCPrice}}<br/>
          </div>
        </b-col>
      </b-row>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'app',
  data: () => ({
    BTCPrice: 0,
    ETHPrice: 0,
    LTCPrice: 0,
    errors: []
  }),
  created: function () {
    this.getEverything()
    setInterval(function () {
      this.getEverything()
      console.log('updated')
    }.bind(this), 1000)
  },
  methods: {
    getEverything: function () {
      this.getBTCPrice()
      this.getETHPrice()
      this.getLTCPrice()
    },
    getBTCPrice: function () {
      axios.get(`https://api.coinbase.com/v2/prices/BTC-USD/spot`)
      .then(response => {
        this.BTCPrice = response.data.data.amount
      })
      .catch(e => {
        this.errors.push(e)
      })
    },
    getETHPrice: function () {
      axios.get(`https://api.coinbase.com/v2/prices/ETH-USD/spot`)
      .then(response => {
        this.ETHPrice = response.data.data.amount
      })
      .catch(e => {
        this.errors.push(e)
      })
    },
    getLTCPrice: function () {
      axios.get(`https://api.coinbase.com/v2/prices/LTC-USD/spot`)
      .then(response => {
        this.LTCPrice = response.data.data.amount
      })
      .catch(e => {
        this.errors.push(e)
      })
    }
  }
}
</script>

<style>

html, body {
  height: 100%;
}

#app {
  font-family: "HelveticaNeue-Light", "Helvetica Neue Light", "Helvetica Neue", Helvetica, Arial, "Lucida Grande", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  height: 100%;
}

.heart-icon {
  vertical-align: super;
  font-size: 18px;
  padding-left: 4px;
}

.max-height {
  height: 100%
}

.crypto-container {
  width: 100%;
  padding-right: 15px;
  padding-left: 15px;
  margin-right: auto;
  margin-left: auto;
}

.currency-container {
  right: 0px;
  width: 270px;
  position: absolute;
  text-align: left;
}

@media (max-width: 575px){
  .currency-container {
    left: 0px;
    padding-left: 40px;
  }
}

.price-container {
  left: 0px;
  width: 270px;
  position: absolute;
  text-align: left;
  padding-left: 40px;
  letter-spacing: 1px;
}

.column-left {
  display: flex;
  font-size: 40px;
  min-height: 200px;
  align-items: center;
  font-weight: bolder;
  background-color: #fff;
}

.column-right {
  display: flex;
  font-size: 40px;
  min-height: 200px;
  align-items: center;
  font-weight: bolder;
  background-color: #333;
  color: #fff;
}
</style>
