<template>
  <div id="app">
    <router-view></router-view>
  </div>
</template>

<script>
  export default {
    name: 'video-frames-extractor'
  }
</script>

<style>
  @import url('https://fonts.googleapis.com/css?family=Source+Sans+Pro');
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  html, body{
    height: 100%;
  }

  #app{
    height: 100%;
  }

  *:focus{
    outline: none;
  }
</style>
