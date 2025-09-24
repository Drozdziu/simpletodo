<template>
  <!-- <img alt="Vue logo" src="./assets/logo.png">
  <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <LabelComponent></LabelComponent>
    <ListComponent></ListComponent>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import ListComponent from './components/ListComponent.vue'
import LabelComponent from './components/LabelComponent.vue'

export default {
  name: 'App',
  components: {
    // HelloWorld,
    ListComponent,
    LabelComponent
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /* margin-top: 60px; */
  margin: 0;
  padding: 0;
}

</style>
