<template>
  <section class="view">
    <header>
      <button @click.prevent="$emit('back')">Back</button>
      <span>Conversion Settings</span>
    </header>
    <form @submit.prevent="">
      <div>
        <label>Currency:</label>
        <input type="text" placeholder="$" :value="currency" />
      </div>
      <div>
        <label>Quotation:</label>
        <input type="text" placeholder="37" :value="quotation" />
      </div>
      <input type="button" @click.prevent="confirm" value="Confirm" />
    </form>
  </section>
</template>
<script lang="ts">
import { Vue } from 'vue-class-component'

export default class CurrencyConversionView extends Vue {
  public currency = '$'
  public quotation = 37

  public created () : void {
    const uri = window.location.search.substring(1)
    const params = new URLSearchParams(uri)
    const currency = params.get('c')
    const quotation = parseFloat(params.get('q') || '0')
    if (currency)
      this.currency = currency

    if (quotation)
      this.quotation = quotation
  }

  public confirm () : void {
    this.$emit('done', {currency: this.currency, quotation: this.quotation})
  }
}
</script>
