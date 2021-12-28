<template>
  <div class="address-input">
    <input @input="$emit('update:modelValue', $event.target.value)" :value="value" />
    <div><button @click="confirm">Confirm</button></div>
  </div>
</template>
<script lang="ts">
import { Vue } from 'vue-class-component'
import { Prop } from 'vue-property-decorator'

export default class AddressInput extends Vue {
  @Prop(String)
  modelValue = ""

  public value = ''

  public created () {
    this.value = this.modelValue

    const uri = window.location.search.substring(1)
    const params = new URLSearchParams(uri)
    const address = params.get('a')
    if (!address) return;

    this.value = address;
    this.$emit('update:modelValue', this.value)
  }

  public confirm () {
    this.$emit('confirm')
  }
}

</script>
