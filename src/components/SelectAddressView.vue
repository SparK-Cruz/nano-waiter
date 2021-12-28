<template>
  <section class="view">
    <header>
      <span><!-- spacer --></span>
      <span>Receiving Address</span>
    </header>
    <AddressInput v-model="address" @confirm="confirm" />
    <div class="old-address" v-for="old in usedAddresses" :key="old" @click.prevent="confirm(old)">{{ old }}</div>
  </section>
</template>
<script lang="ts">
import { Options, Vue } from 'vue-class-component'
import AddressInput from './AddressInput.vue'

@Options({
  components: {
    AddressInput
  }
})
export default class SelectAddressView extends Vue {
  public address = ''
  public usedAddresses: string[] = []

  public confirm () : void {
    this.$emit('done', this.address)
  }
}
</script>
