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
            <span class="btc-container">
              ${{BTCPrice}}
            </span><br/>
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
    BTCPrice: '',
    ETHPrice: '',
    LTCPrice: '',
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
    convertCurrency: function (num) {
      // let newNum = num.toString()
      // console.log(newNum)
      if (num) {
        return num.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',')
      } else {
        return 0
      }
    },
    getEverything: function () {
      this.getBTCPrice()
      this.getETHPrice()
      this.getLTCPrice()
    },
    getBTCPrice: function () {
      axios.get(`https://api.coinbase.com/v2/prices/BTC-USD/spot`)
      .then(response => {
        this.BTCPrice = this.convertCurrency(response.data.data.amount)
      })
      .catch(e => {
        this.errors.push(e)
      })
    },
    getETHPrice: function () {
      axios.get(`https://api.coinbase.com/v2/prices/ETH-USD/spot`)
      .then(response => {
        this.ETHPrice = this.convertCurrency(response.data.data.amount)
      })
      .catch(e => {
        this.errors.push(e)
      })
    },
    getLTCPrice: function () {
      axios.get(`https://api.coinbase.com/v2/prices/LTC-USD/spot`)
      .then(response => {
        this.LTCPrice = this.convertCurrency(response.data.data.amount)
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

.btc-container {
  background-color: #111;
  padding: 2px 0px 2px 12px;
  margin-left: -12px;
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
