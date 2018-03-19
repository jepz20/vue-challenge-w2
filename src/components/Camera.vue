<template>
  <div v-show="isActive"><slot></slot></div>
</template>

<script>
export default {
  name: 'Camera',
  data: () => ({
    isActive: false
  }),
  props: {
    id: String,
    active: { default: false }
  },
  mounted() {
    this.isActive=this.active
  }
}
</script>
<style scoped>
  .tab {
    flex: 1;
    justify-content: center
  }
</style>