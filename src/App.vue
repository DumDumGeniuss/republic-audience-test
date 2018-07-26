<template>
  <div id="app">
    <UploadNewImagePage/>
  </div>
</template>

<script>
import UploadNewImagePage from './components/UploadNewImagePage.vue'

export default {
  name: 'app',
  components: {
    UploadNewImagePage
  }
}
</script>

<style>
#app {
  font-family: PingFangTC-Regular;
  width: 100%;
  overflow-x: hidden;
}
body {
  margin: 0;
  padding: 0;
  width: 100vw;
}
* {
  box-sizing: border-box;
}
</style>
