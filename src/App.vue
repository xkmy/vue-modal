<template>
  <div id="app">
    <div class="show" @click="onShow">show modal</div>

    <Modal
      :visible="visible"
      :animation="animation"
      width="400"
      centered
      :duration="200"
      className="my-dialog"
      @onOk="onOk"
      @onCancel="onCancel"
      title="标题"
    >
      <div class="body">A vue modal.</div>
      <div class="body">A vue modal.</div>
      <div class="body">A vue modal.</div>
    </Modal>
  </div>
</template>

<script>
import Modal from "./components/Modal/Modal";
export default {
  name: "Home",
  components: { Modal },
  data() {
    return {
      visible: false,
      animation: "slideDown", // 'fade' || 'zoom' || 'slideDown'
    };
  },
  methods: {
    onShow() {
      this.visible = true;
    },
    onOk() {
      this.visible = false;
    },
    onCancel() {
      this.visible = false;
    },
  },
};
</script>

<style lang="scss">
#app {
  height: 100%;

  .show {
    cursor: pointer;
  }
}
</style>
