<template>
  <div id="app">
    <Container v-bind="containers[0]"/>
    <Channel v-bind="channel"/>
    <Container v-bind="containers[1]"/>
  </div>
</template>

<script>
import Container from './components/Container.vue'
import Channel from './components/Channel.vue'
import Vue from 'vue'

window.bus = new Vue()

const App = {
  name: 'app',
  components: {
    Container,
    Channel
  },
  data: () => ({
    containers: [{
      id: 'A',
      waterLevel: 20,
      channelLevel: 20,
      containerLevel: 40,
    },
    {
      id: 'B',
      waterLevel: 40,
      channelLevel: 60,
      containerLevel: 0
    }],
    channel: {
      containerLevel: 60,
      leftId: 'A',
      rightId: 'B'
    }
  })
}

export default App
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 20px;
  height: 200px;
  display: flex;
  align-items: flex-end;
  position: relative;

}
.row {
  float: left;
}

.liquid {
    display: flex;
    flex-direction: column-reverse;
    width: 40px;
    height: 100px;
    background-color: blue;
    transition: width 500ms, height 1s;
    color: white;
    text-align: center;
    font-size: 14px;
}
</style>
