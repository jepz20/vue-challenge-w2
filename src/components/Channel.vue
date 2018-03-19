<template>
<section class="row" :style="{marginBottom: `${containerLevel}px`}">
  <div class="channel">
    <div class="liquid" :style="{height: this.level + 'px'}">
    </div>
  </div>
</section>
</template>

<script>
import { eventBus } from '../main'
const MAX_LEVEL = 20

export default {
  name: 'Container',
  props: {
    containerLevel: Number,
    leftId: String,
    rightId: String
  },
  data() {
    return {
      left: {
        id: this.leftId,
        isAtLevel: false,
        isFull: false
      },
      right: {
        id: this.rightId,
        isAtLevel: false,
        isFull: false
      },
      level: 0
    }
  },
  computed: {
    containerAtLevel() {
      return this.left.isAtLevel && this.right.isAtLevel
    }
  },
  created() {
    eventBus.$on('isAtChannelLevel', (id) => {
      if (this.left.id === id) this.left.isAtLevel = true
      if (this.right.id === id) this.right.isAtLevel = true
    })

    eventBus.$on('isFull', (id) => {
      if (this.left.id === id) this.left.isFull = true
      if (this.right.id === id) this.right.isFull = true
    })

    eventBus.$on('levelWater', (containerInfo) => {
      let sendTo
      let receiveFrom
      const isLeft = containerInfo.id === this.left.id
      const isRight = containerInfo.id === this.right.id
      let distributeVal = 0
      let sendToVal = containerInfo.value
      let receiveFromVal = containerInfo.value

      if (isLeft) {
        sendTo = this.right
        receiveFrom = this.left
      } else if(isRight) {
        sendTo = this.left
        receiveFrom = this.right
      } else return



      if (sendTo.isFull) return

      if (this.containerAtLevel) {
        if (this.level < MAX_LEVEL) {
          distributeVal = containerInfo.value / 3
          this.level += distributeVal
        } else {
          distributeVal = containerInfo.value / 2
        }
        sendToVal = distributeVal
        receiveFromVal = containerInfo.value - distributeVal
      }

      eventBus.$emit('channelDecrement', {
        id: receiveFrom.id,
        value: receiveFromVal
      })

      eventBus.$emit('channelIncrement', {
        id: sendTo.id,
        value: sendToVal
      })
    })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.channel {
    display: flex;
    width: 40px;
    height: 20px;
    background-color: #eee;
    flex-direction: column-reverse;
    transition: width 500ms, height 1s;
    margin-top: 100px;
}
</style>
