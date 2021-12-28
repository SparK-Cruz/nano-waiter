<template>
<section id="app">
  <header><h1>Nano Waiter</h1></header>
  <SelectAddressView @done="setupAddress" v-if="mode == 'address'" />
  <CurrencyConversionView @done="setupCurrency" @back="mode = 'address'" v-if="mode == 'currency'" />
  <OperationView
      @back="mode = 'currency'"
      :currency="currency"
      :quotation="quotation"
      :address="address"
      v-if="mode == 'operation'" />
</section>
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component'
import SelectAddressView from './components/SelectAddressView.vue'
import CurrencyConversionView from './components/CurrencyConversionView.vue'
import OperationView from './components/OperationView.vue'

enum Mode {
  ADDRESS = 'address',
  CURRENCY = 'currency',
  OPERATION = 'operation',
}

@Options({
  components: {
    SelectAddressView,
    CurrencyConversionView,
    OperationView
  }
})
export default class App extends Vue {
  public mode: Mode = Mode.ADDRESS
  public address = ''
  public quotation = 37
  public currency = '$'

  public setupAddress (address: string) : void {
    this.address = address
    this.mode = Mode.CURRENCY
  }

  public setupCurrency (config: { currency: string, quotation: number }) : void {
    this.currency = config.currency
    this.quotation = config.quotation

    this.mode = Mode.OPERATION
  }
}
</script>

<style>
body, html {
  margin: 0px;
  padding: 0px;
}

#app {
  font-family: 'Roboto Sans', 'Liberation Sans', 'Arial', sans-serif;
  color: #000034;
}

#app > header {
  padding: 20px;
}

.view {
  max-width: 310px;
  margin: 0px auto;
  height: 100%;
}
.view > *, .view > * label {
  width: 100%;
  display: block;
}
.view header {
  margin-bottom: 20px;
}
.view label {
  font-size: small;
  margin-bottom: 2px;
}

.view header span {
  font-size: large;
  padding: 0px 20px;
}

.view input, .view button {
  padding: 7px 12px;
  border-radius: 3px;
  border: 1px solid black;
}
.view :not(header) input, .view :not(header) button {
  min-width: 300px;
  box-sizing: border-box;
  margin-bottom: 10px;
}
</style>
