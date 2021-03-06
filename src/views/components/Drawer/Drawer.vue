
<template>
  <transition name="slide" mode="out-in">
    <div v-show="visible" class="drawer-wrapper" :style="{width}">
      <div class="drawer-header">
        <h4>{{title}}</h4>
        <i class="el-icon-close" @click="cancel"></i>
      </div>
      <div class="drawer-main">
        <slot></slot>
      </div>
      <div class="drawer-footer">
        <el-button type="primary" @click="confirm">{{confirmStr || '确定'}}</el-button>
        <el-button class="cancel" @click="cancel">{{cancelStr || '取消'}}</el-button>
      </div>
    </div>
  </transition>
</template>

<script>
import Popup from 'element-ui/lib/utils/popup'

export default {
  name: 'BaseDrawer',

  mixins: [Popup],

  props: {
    modal: {
      type: Boolean,
      default: true
    },

    closeOnClickModal: {
      type: Boolean,
      default: true
    },

    closeOnPressEscape: {
      type: Boolean,
      default: true
    },

    beforeClose: Function,

    width: String,

    title: String,
    confirmStr: String,
    cancelStr: String
  },

  mounted () {
    if (this.visible) {
      this.rendered = true
      this.open()
    }
  },

  watch: {
    visible (val) {
      if (val) {
        this.$emit('open')
      } else {
        this.onClose()
      }
    }
  },

  methods: {
    onClose () {
      if (this.beforeClose) {
        this.beforeClose(this.hide)
      } else {
        this.hide()
      }
    },

    hide () {
      this.$emit('update:visible', false)
      this.$emit('close')
    },

    confirm () {
      this.$emit('confirm')
    },

    cancel () {
      this.$emit('cancel')
    }
  }
}
</script>

<style scoped lang="scss">
.drawer {
  &-wrapper {
    position: fixed;
    top: 0;
    right: 0;
    bottom: 0;
    display: flex;
    flex-direction: column;
    min-width: 300px;
    overflow: hidden;
    background: #ffffff;
  }

  &-header {
    position: relative;
    flex: none;
    padding: 15px;
    border-bottom: 1px solid #e0e0e0;

    > h4 {
      margin: 0;
      font-size: 14px;
    }

    .el-icon-close {
      position: absolute;
      top: 50%;
        transform: translateY(-50%);
      right: 20px;
        cursor: pointer;
      font-size: 18px;
    }
  }

  &-main {
    flex: 1;
    padding: 25px 15px 20px;
    overflow: auto;
  }

  &-footer {
    flex: none;
    padding: 10px 15px;
    border-top: 1px solid #e0e0e0;
    text-align: center;
    .cancel{
      margin-left: 50px;
    }
  }
}

.slide {
  &-enter-active {
    animation: slide-fade-in 0.3s;
  }

  &-leave-active {
    animation: slide-fade-out 0.3s;
  }
}

@keyframes slide-fade-in {
  0% {
    transform: translate3d(20px, 0, 0);
    opacity: 0;
  }
  100% {
    transform: translate3d(0, 0, 0);
    opacity: 1;
  }
}

@keyframes slide-fade-out {
  0% {
    transform: translate3d(0, 0, 0);
    opacity: 1;
  }
  100% {
    transform: translate3d(20px, 0, 0);
    opacity: 0;
  }
}
</style>
