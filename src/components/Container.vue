<template>
<section class="row" >
  <button class="action" @click="incrementLevel(addDefault)">+ {{id}}</button>
  <div class="container" :style="{marginBottom: `${containerLevel}px`}">
    <div class="liquid" :style="{height: level + 'px'}"></div>
  </div>
</section>
</template>

<script>
import { eventBus } from '../main'

const MAX_LEVEL = 100
export default {
  name: 'Container',
  props: {
    waterLevel: Number,
    containerLevel: Number,
    id: String,
    channelLevel: Number
  },
  data: function() {
    return {
      level: this.waterLevel,
      addDefault: 20
    }
  },
  created() {
    eventBus.$on('channelIncrement', (data) => {
      if (data.id === this.id) {
        this.changeLevel(data.value)
      }
    })

    eventBus.$on('channelDecrement', (data) => {
      if (data.id === this.id) {
        this.changeLevel(data.value*-1)
      }
    })
  },
  mounted() {
    this.checkIsFull(),
    this.checkIsAtChannelLevel()
  },
  methods: {
    checkIsFull() {
      if (this.level >= MAX_LEVEL) {
        eventBus.$emit('isFull', this.id)
      }
    },
    checkIsAtChannelLevel() {
      if (this.level >= this.channelLevel) {
        eventBus.$emit('isAtChannelLevel', this.id)
      }
    },
    changeLevel (value) {
      const nextLevel = this.level + value
     if (this.nextLevel < 0) {
        this.level = 0
      } else {
        this.level = nextLevel
      }
      this.checkIsFull()
      this.checkIsAtChannelLevel()
      console.log(this.level, this.channelLevel, this.id)
    },
    incrementLevel (value=20) {
      this.changeLevel(value)
      console.log(this.level,Math.floor(this.level), 'ULTIMO')
      if (Math.floor(this.level) > MAX_LEVEL) {
        this.level = MAX_LEVEL
        return
      }
      if (this.level > this.channelLevel) {
        setTimeout(() =>
        {
          eventBus.$emit('levelWater', {
            level: this.level,
            value,
            id: this.id
          })
        }, 500)
      }
      // setTimeout(this.levelWater, 500)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .action {
    width: 50px;
    top: 25px;
    margin-left: -20px;
    position: absolute;
  }
  .container {
      display: flex;
      flex-direction: column-reverse;
      width: 40px;
      height: 100px;
      background-color: #eee;
      margin-top: 20px;
      transition: width 500ms, height 1s;
  }
</style>
