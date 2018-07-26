<template>
  <div>
    <div class="title-bar">
      <h1 class="title">{{ title }}</h1>
      <span class="date-text">{{ dateText }}</span>
    </div>
    <div class="theme-section">
      <img class="theme-section-bg-img" src="../assets/cake.jpg" />
      <div class="profile-photo-frame">
        <img class="profile-photo" src="../assets/josh.png" />
      </div>
    </div>
    <div class="control-panel">
      <ScrollBar
        :title="'Brightness'"
        :description="'Slide to adjust image brightness!☀️'"
        :mainColor="'rgba(37, 169, 91, 1)'"
        :subColor="'rgba(37, 169, 91, 0.25)'"
        :defaultValue="brightness"
        :minValue="-100"
        :maxValue="100"
        :onValueChange="changeBrightness"
        :disabled="!hasImage"
      />
      <ScrollBar
        :title="'Contrast'"
        :description="'Slide to adjust image contrast! 🌓'"
        :mainColor="'rgba(74, 144, 226, 1)'"
        :subColor="'rgba(74, 144, 226, 0.25)'" 
        :defaultValue="contrast"
        :minValue="-100"
        :maxValue="100"
        :onValueChange="changeContrast"
        :disabled="!hasImage"
        style="margin-top: 7px;"
      />
      <ImageUploadPanel
        ref="image-upload-panel"
        :onImageUploaded="setupImage"
        style="margin-top: 29px;"
      />
    </div>
  </div>
</template>

<script>
import ScrollBar from './ScrollBar.vue'
import ImageUploadPanel from './ImageUploadPanel.vue'

export default {
  name: 'UploadNewImagePage',
  props: {},
  data () {
    return {
      title: 'Brightness & Contrast Developer Test',
      dateText: '01 Jun, 2018 – 31 Dec, 2019',
      brightness: 0,
      contrast: 0,
      hasImage: false,
    }
  },
  mounted: function() {
    this.imageUploadPanel = this.$refs['image-upload-panel'];
  },
  components: {
    ScrollBar,
    ImageUploadPanel
  },
  methods: {
    setupImage() {
      this.hasImage = true;
      this.imageUploadPanel.decorateImage({
        brightness: this.brightness,
        contrast: this.contrast,
      });
    },
    changeBrightness(value) {
      this.brightness = value;
      this.imageUploadPanel.decorateImage({
        brightness: this.brightness,
        contrast: this.contrast,
      });
    },
    changeContrast(value) {
      this.contrast = value;
      this.imageUploadPanel.decorateImage({
        brightness: this.brightness,
        contrast: this.contrast,
      });
    },

  }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.title-bar {
  padding: 11px 20px 15px;
  background: #7344C0;
}
.title {
  white-space: pre;
  font-size: 15px;
  color: white;
  letter-spacing: 0.05em;
  text-align: center;
  font-weight: 300;
  line-height: 25px;
  margin: 0;
}
.date-text {
  display: block;
  font-size: 13px;
  color: #FFFFFF;
  letter-spacing: 0;
  line-height: 20px;
  text-align: center;
}
.theme-section {
  position: relative;
}
.theme-section-bg-img {
  width: 100vw;
  margin-left: 50%;
  transform: translateX(-50%);
}
.profile-photo-frame {
  position: absolute;
  width: 60px;
  height: 60px;
  bottom: -8px;
  left: 50%;
  transform: translateX(-50%);
  border: 2px solid white;
  border-radius: 30px;
}
.profile-photo {
  width: 100%;
  height: 100%;
}
.control-panel {
  padding: 17px 20px 20px;
}
.image-upload-frame {
  border: 1px solid #DCDEE4;
  border-radius: 5px;
  margin-top: 29px;
}
.image-upload-control-panel {
  display: flex;
  padding: 18px 8px 8px;
}
.image-name-section {
  border: 1px solid #DCDEE4;
  border-radius: 5px;
}
.image-name-section-name {
  
}
</style>
