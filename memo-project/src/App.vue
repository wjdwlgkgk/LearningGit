<template>
  <div id="app">
    <!-- <test-pr></test-pr> -->
    <appHeader></appHeader>
    <br/>
    <memo></memo>
  </div>
</template>
<script>

import appHeader from './components/appheader.vue'
import memo from './components/memo.vue'

export default {
  name: 'App',
  components :{
    appHeader,
    memo,
    // testPr
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
