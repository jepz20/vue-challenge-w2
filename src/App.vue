<template>
  <div id="app">
    <cameras>
      <camera :key="container.id" v-for="container in containers" :id="container.id">
        <container v-bind="container" />
      </camera>
    </cameras>
    <channel v-show="false" v-bind="channel"/>
  </div>
</template>

<script>
import Container from './components/Container.vue'
import Channel from './components/Channel.vue'
import Cameras from './components/Cameras.vue'
import Camera from './components/Camera.vue'

const App = {
  name: 'app',
  components: {
    Container,
    Channel,
    Cameras,
    Camera
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
  max-width: 300px;
  display: flex;
  align-items: flex-end;
  position: relative;
  background-color:	#b2d8d8;

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
.tabs {
  position: absolute;
  top: 0;
  display: flex;
  width: 100%;
  background: #696969;
  left: 0;
}
.tab {
  flex: 1;
  justify-content: center
}

.active {
  background-color:	#b2d8d8;
}
</style>
