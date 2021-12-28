<template>
  <section class="view">
    <header>
      <button @click.prevent="$emit('back')">Back</button>
      <span>Point of Sale</span>
      <button @click.prevent="clone">Clone Config</button>
    </header>
    <label>Price:</label>
    <form @submit.prevent="pay">
      <span class="currency">
        <input type="number" step="0.01" placeholder="0.00" @input="convert($event.target.value)" />
        <span class="symbol">{{currency}}</span>
      </span>
      <div>
        <input type="submit" value="Confirm" />
      </div>
    </form>
    <Modal ref="pay">
      <div id="qr" class="qr"></div>
      <p class="price">Ӿ{{ price }}</p>
    </Modal>
    <Modal ref="config">
      <div id="config-qr" class="qr"></div>
      <p class="price">{{ shortAddress }}</p>
      <p class="price">{{ currency }} {{ quotation }}</p>
      <p class="price"></p>
    </Modal>
  </section>
</template>
<script lang="ts">
import { Options, Vue } from 'vue-class-component'
import { Prop, Watch } from 'vue-property-decorator'
import Modal from './Modal.vue'

const PRECISION = 6
const RAW_DECIMALS = 30

@Options({
  components: {
    Modal
  }
})
export default class OperationView extends Vue {
  @Prop(String)
  public address = '';
  @Prop(String)
  public currency = '';
  @Prop(Number)
  public quotation = 0;

  @Watch("shortAddress")
  public get shortAddress () : string {
    return this.address.substr(0, 12) + '...' + this.address.substr(-6)
  }

  public localprice = 0
  public price = 0

  public created () : void {
  }

  public convert (localprice : number) : void {
    this.price = parseFloat((localprice / this.quotation).toFixed(PRECISION))
  }

  public pay () : void {
    (<any>this.$refs.pay).open()
    setTimeout(() => {
      this.renderQr()
    }, 0)
  }

  public clone () : void {
    (<any>this.$refs.config).open()
    setTimeout(() => {
      this.renderConfigQr()
    }, 0)
  }

  private renderQr () : void {
    const QRCode = (<any>window).QRCode
    const qrelement : HTMLDivElement = (<HTMLDivElement>document.getElementById('qr'));
    qrelement.innerHTML = ''
    new QRCode(qrelement, {
        text: this.generatePayload(),
        width: 200,
        height: 200,
        colorDark : "#000034",
        colorLight : "#F4FAFF",
        correctLevel : QRCode.CorrectLevel.H
    });
  }

  private renderConfigQr () : void {
    const url = document.location.href.split('?')[0]

    const QRCode = (<any>window).QRCode
    const qrelement : HTMLDivElement = (<HTMLDivElement>document.getElementById('config-qr'));
    qrelement.innerHTML = ''
    new QRCode(qrelement, {
        text: url + '?a=' + this.address + '&c=' + this.currency + '&q=' + this.quotation,
        width: 200,
        height: 200,
        colorDark : "#000034",
        colorLight : "#F4FAFF",
        correctLevel : QRCode.CorrectLevel.H
    });
  }

  private generatePayload () : string {
    let result = 'nano:' + this.address
    if (this.price > 0)
      result += '?amount=' + this.toNanoRaw(this.price)
    return result
  }

  private toNanoRaw (value : number) : string {
    return value.toFixed(PRECISION).replace('.', '') + Array(RAW_DECIMALS - PRECISION).fill('0').join('')
  }
}
</script>
<style scoped>
  .currency {
    position: relative;
    height: fit-content;
  }
  .currency .symbol {
    position: absolute;
    margin-top: -6px;
    font-size: 13px;
    top: 0px;
    left: 0px;
    width: 20px;
    padding: 7px 12px;
    background-color: #3c4f7a;
    color: white;
    border: 1px solid black;
    border-right: none;
    border-top-left-radius: 3px;
    border-bottom-left-radius: 3px;
  }
  .currency input {
    padding-left: 56px;
  }

  .qr {
    margin: 50px auto;
    width: 200px;
  }
  .price {
    font-size: 48px;
    color: #20204c;
    text-align: center;
  }
</style>
