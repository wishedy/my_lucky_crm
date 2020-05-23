<template>
    <BaseDrawer
        :visible="visible"
        width="570px"
        @close="handleClose"
        @cancel="handleClose"
    >

    </BaseDrawer>
</template>
<script>
import BaseDrawer from '@/views/components/Drawer/Drawer.vue'
export default {
  props: {
    visible: {
      type: Boolean,
      required: true,
      default () {
        return false
      }
    }
  },
  name: 'edit',
  data () {
    return {
      direction: 'rtl'
    }
  },
  components: {
    BaseDrawer
  },
  methods: {
    handleClose () {
      const _this = this
      _this.$emit('update:visible', false)
    }
  }
}
</script>
<style lang="scss" scoped>
    .edit-panel{
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
</style>
