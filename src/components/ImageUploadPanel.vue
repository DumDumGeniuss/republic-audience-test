<template>
  <div class="wrapper">
    <canvas width="400" height="200" ref="canvas" class="canvas">

    </canvas>
    <div class="control-panel">
      <div class="name-section">
        <div class="name-section-label">
          NAME
        </div>
        <div class="name-section-content">
          {{ fileName }}
        </div>
      </div>
      <div class="upload-button" @click="handleButtonClick">
        <img class="upload-button-triangle-icon" src="../assets/triangle.png" />
        <span class="upload-button-text">UPLOAD</span>
      </div>
      <input ref="image-upload" type="file" v-bind:style="{ display: 'none' }" />
    </div>
  </div>
</template>

<script>
export default {
  name: 'ImageUploadPanel',
  props: {
    onImageUploaded: Function,
  },
  data () {
    return {
      fileName: 'Please Upload Image...',
      originImageValue: null,
    }
  },
  mounted: function () {
    this.imageUploadElem = this.$refs['image-upload'];
    this.imageUploadElem.onchange = this.embedImageToCanvas;

    // initial canvas
    this.canvasElem = this.$refs['canvas'];
    const context = this.canvasElem.getContext('2d');
    context.font = '24px PingFangTC-Regular';
    context.textAlign = 'center';
    context.fillStyle = '#8392A6';
    context.fillText('No picutre uploaded', this.canvasElem.width / 2, this.canvasElem.height / 2);
    this.originImageValue = context.getImageData(0, 0, 400, 200);
  },
  methods: {
    decorateImage: function (params) {
      const brightness = params.brightness;
      const contrast = params.contrast;
      const context = this.canvasElem.getContext('2d');
      const imageData = context.getImageData(0, 0, 400, 200);

      // Birghtness
      for (let i = 0; i < imageData.data.length; i += 4) {
        imageData.data[i] = this.originImageValue.data[i] + parseInt(255 * (brightness / 100), 10);
        imageData.data[i + 1] = this.originImageValue.data[i + 1] + parseInt(255 * (brightness / 100), 10);
        imageData.data[i + 2] = this.originImageValue.data[i + 2] + parseInt(255 * (brightness / 100), 10);
      }

      const factor = (255 + contrast * 2.55) / (255.01 - contrast * 2.55);
      // Constrast
      for (let i = 0; i < imageData.data.length; i += 4) {
        imageData.data[i] = factor * (imageData.data[i] - 128) + 128;
        imageData.data[i + 1] = factor * (imageData.data[i + 1] - 128) + 128;
        imageData.data[i + 2] = factor * (imageData.data[i + 2] - 128) + 128;
      }

      context.putImageData(imageData, 0, 0);
    },
    embedImageToCanvas: function () {
      const fileReader = new FileReader();
      fileReader.onload = (event) => {
        this.fileName = this.imageUploadElem.files[0].name;
        const img = new Image();
        img.onload = () => {
          const imgHeight = img.height * (this.canvasElem.width / img.width);
          const context = this.canvasElem.getContext('2d');
          context.drawImage(img, 0, 0, this.canvasElem.width, imgHeight);
          this.originImageValue = context.getImageData(0, 0, 400, 200);
          this.onImageUploaded();
        }
        img.src = event.target.result;
      }
      fileReader.readAsDataURL(this.imageUploadElem.files[0]);
    },
    handleButtonClick: function () {
      this.imageUploadElem.click();
    }
  }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.wrapper {
  border: 1px solid #DCDEE4;
  border-radius: 5px;
  overflow: hidden;
}
.canvas {
  width: 100%;
  border-bottom: 1px solid #DCDEE4;
  background: #F6F8FA;
}
.control-panel {
  display: flex;
  padding: 18px 8px 8px;
}
.name-section {
  display: inline-block;
  width: calc(100% - 111px);
  border: 1px solid #DCDEE4;
  border-radius: 5px;
  height: 31px;
  overflow: hidden;
}
.name-section-label {
  display: inline-block;
  border-right: 1px solid #DCDEE4;
  background: #F6F8FA;
  width: 60px;
  height: 31px;
  text-align: center;
  font-size: 11px;
  color: #8392A6;
  letter-spacing: 1.1px;
  line-height: 31px;
}
.name-section-content {
  display: inline-block;
  width: calc(100% - 60px);
  padding-left: 6px;
  font-size: 11px;
  white-space: nowrap;
  color: #25A95B;
  letter-spacing: 1.1px;
  text-align: center;
  line-height: 31px;
}
.upload-button {
  display: inline-flex;
  align-items: center;
  justify-content: space-around;
  margin-left: 24px;
  padding: 0 4px;
  width: 87px;
  height: 31px;
  background: #F6F8FA;
  border: 1px solid #DCDEE4;
  border-radius: 5px;
  cursor: pointer;
}
.upload-button-triangle-icon {
  display: inline-block;
  width: 10px;
  height: 10px;
  transform: translateY(1px);
}
.upload-button-text {
  display: inline-block;
  font-size: 12px;
  color: #4A90E2;
  letter-spacing: 0.55px;
}
</style>
