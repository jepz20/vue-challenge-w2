<template>
  <div class="cameras">
    <div class="titles">
      <span
        v-for="camera in cameras"
        :key="camera.id"
        class="camera"
        :class="{'active': camera.isActive}"
        @click= "selectCamera(camera.id)"
        >
        Camera {{camera.id}}
      </span>
    </div>
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: 'Cameras',
  data: () => ({
    active: 'A',
    cameras: []
  }),
  methods: {
    selectCamera(id) {
      this.cameras.forEach(camera => {
          camera.isActive = camera.id===id
      })
    }
  },
  created() {
    this.cameras = this.$children
  },
  mounted() {
    if (this.cameras.length) {
      const hasActive = this.cameras.some(it => it.isActive)
      if (!hasActive) this.cameras[0].isActive = true
    }
  }
}
</script>
<style scoped>
  .cameras {
    width: 100%;
    display: flex;
    justify-content: center;
  }
  .titles {
    position: absolute;
    top: 0;
    display: flex;
    width: 100%;
    background: #008080;
    left: 0;
  }
  .camera {
    flex: 1;
    justify-content: center;
    height: 30px;
    line-height: 30px;
    width: 100%;
    color: #b2d8d8
  }

  .active {
    background-color:	#b2d8d8;
    color: #004c4c
  }
</style>