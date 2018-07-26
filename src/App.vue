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
}
body {
  margin: 0;
  padding: 0;
  overflow-x: hidden;
}
* {
  box-sizing: border-box;
}
</style>
