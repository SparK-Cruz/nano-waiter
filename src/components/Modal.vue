<template>
  <section v-if="isOpen" class="modal-container" @click.stop="close">
    <div ref="content" class="content" @click.stop="">
      <div class="modal-x-button" @click.stop="close" />
      <slot></slot>
    </div>
  </section>
</template>
<script lang="ts">
import { Vue } from 'vue-class-component'

export default class Modal extends Vue {
  public isOpen = false

  public open () : void {
    this.isOpen = true
  }

  public close () : void {
    this.isOpen = false
  }
}
</script>
<style scoped>
.modal-container {
    position: fixed;
    top: 0px;
    left: 0px;
    width: 100%;
    height: 100%;
    overflow: auto;
    z-index: 2;
    cursor: default;
    background-color: rgba(32, 32, 32, 0.2);
}
.modal-container .modal-x-button {
    height: 26px;
    width: 26px;
    border-radius: 13px;
    font-size: 20px;
    line-height: 26px;
    background-color: #4A90E2;
    color: white;
    position: absolute;
    top: 5px;
    left: calc(100% - 30px);
    text-align: center;
    cursor: pointer;
}
.modal-container .modal-x-button:after {
    position: relative;
    content: '×';
}
.modal-container .content {
    /* position: absolute; */
    width: 92%;
    min-height: 90%;
    margin: 5% auto;
    background-color: white;
    border: 1px solid #4A90E2;
    border-radius: 5px;
    z-index: 3;
}
</style>
