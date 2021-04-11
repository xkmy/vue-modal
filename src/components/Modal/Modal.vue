<template>
  <transition name="modal-fade">
    <div
      v-show="visible"
      :style="style"
      :class="['modal', className]"
      @keyup.esc="onEsc"
    >
      <div
        class="modal-mask"
        v-if="mask"
        @click="onClickMask"
        :style="customMaskStyles"
      />
      <transition :name="`modal-${animation}`">
        <div :class="centered ? 'modal-centered' : null">
          <div class="modal-dialog" v-show="visible" :style="dialogStyle">
            <div class="modal-header">
              <span class="modal-close" v-if="closeButton" @click="onCancel" />
              <div class="modal-title">{{ title }}</div>
            </div>

            <div class="modal-body">
              <slot></slot>
            </div>

            <slot name="footer">
              <div class="modal-footer">
                <button class="modal-confirm-btn" @click="onOk">ok</button>
                <button class="modal-cancel-btn" @click="onCancel">
                  close
                </button>
              </div>
            </slot>
          </div>
        </div>
      </transition>
    </div>
  </transition>
</template>

<script>
export default {
  name: "modal",
  props: {
    visible: {
      type: Boolean,
      required: true,
    },
    title: {
      type: String,
      default: "",
    },
    width: {
      type: String,
      default: "400",
    },
    height: {
      type: String,
    },
    duration: {
      type: Number,
      default: 300,
    },
    measure: {
      type: String,
      default: "px",
    },
    animation: {
      type: String,
      default: "zoom",
    },
    mask: {
      type: Boolean,
      default: true,
    },
    closeButton: {
      type: Boolean,
      default: true,
    },
    closeOnEsc: {
      type: Boolean,
      default: false,
    },
    closeOnClickMask: {
      type: Boolean,
      default: true,
    },
    centered: {
      type: Boolean,
      default: false,
    },
    className: {
      type: String,
      default: "",
    },
    customStyles: {
      type: Object,
      default: () => ({}),
    },
    customMaskStyles: {
      type: Object,
      default: () => ({}),
    },
  },
  computed: {
    style() {
      return {
        animationDuration: `${this.duration}ms`,
      };
    },
    dialogStyle() {
      const width = this.hasUnit(this.width) ? this.width : this.width + "px";
      const height = this.hasUnit(this.height)
        ? this.height
        : this.height + "px";

      if (this.centered) {
        return {
          width,
          height,
          top: 0,
          ...this.customStyles,
        };
      }
      return {
        width,
        height,
        animationDuration: `${this.duration}ms`,
        ...this.customStyles,
      };
    },
  },
  watch: {
    visible(isShow) {
      isShow &&
        this.$nextTick(() => {
          this.$el.focus();
        });
    },
  },
  methods: {
    hasUnit(val) {
      const reg = /[a-zA-Z]/i;
      return reg.test(val);
    },
    onEsc() {
      if (this.visible && this.closeOnEsc) {
        this.$emit("onCancel");
      }
    },
    onClickMask() {
      this.$emit("clickMask");
      if (this.closeOnClickMask) {
        this.onCancel();
      }
    },
    onCancel() {
      this.$emit("onCancel");
    },
    onOk() {
      this.$emit("onOk");
    },
  },
};
</script>

<style lang='scss'>
@import "./modal.scss";
</style>