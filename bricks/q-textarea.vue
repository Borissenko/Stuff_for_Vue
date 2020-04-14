<template>
  <textarea
    class="area wide"
    :disabled="disabled"
    :style="{ height: height + 'px' }"
  ></textarea>
</template>

<script>
export default {
  name: 'QBtn',
  props: {
    disabled: {
      type: Boolean,
      default: false
    },
    height: {
      type: String,
      default: '100'
    }
  }
}
</script>
<style lang="scss" scoped>
.area {
  width: 100%;
  height: 100%;
  padding: 20px 40px;
  outline: none;
  border: 2px solid $secondary;
  resize: none;
  &:disabled {
    opacity: 0.5;
  }
}
</style>
