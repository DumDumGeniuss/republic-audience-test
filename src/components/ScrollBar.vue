<template>
  <div class="wrapper" v-bind:style="{ cursor: disabled ? 'not-allowed' : '', opacity: disabled ? '0.6' : '1' }">
    <span class="title" v-bind:style="{ color: mainColor }">{{ title }}</span>
    <div
      class="scroll-section"
      ref="scroll-section"
      @mousedown="startScrolling"
      @touchstart="startScrolling"
      v-bind:style="{ cursor: disabled ? '' : 'pointer' }"
    >
      <div class="light-bar" v-bind:style="{ background: subColor }" />
      <div class="dark-bar" v-bind:style="{ background: mainColor, width: `calc(${percentage}% + 10px)` }" />
      <div class="ball" v-bind:style="{ background: mainColor, left: `${percentage}%` }" />
    </div>
    <span class="description">{{ description }}</span>
  </div>
</template>

<script>
export default {
  name: 'ScrollBar',
  props: {
    disabled: Boolean,
    defaultValue: Number,
    maxValue: Number,
    minValue: Number,
    title: String,
    description: String,
    mainColor: String,
    subColor: String,
    onValueChange: Function,
  },
  data() {
    return {
      value: this.defaultValue
    };
  },
  computed: {
    percentage: function () {
      return parseInt((this.value - this.minValue) / (this.maxValue - this.minValue) * 100, 10);
    }
  },
  mounted: function () {
    this.scrollSectionElem = this.$refs['scroll-section'];
  },
  beforeDestroy: function () {
    this.removeScrollListener();
  },
  methods: {
    scrollListener(event) {
      const mouseX = event.clientX || event.touches[0].pageX;
      this.moveBall(mouseX)
    },
    moveBall(mouseX) {
      this.scrollSectionElem = this.$refs['scroll-section'];

      const rect = this.scrollSectionElem.getBoundingClientRect();
      const leftX = rect.left;
      const rightX = leftX + rect.width;
      if (mouseX < leftX) {
        this.value = this.minValue;
      } else if (mouseX > rightX) {
        this.value = this.maxValue;
      } else {
        this.value = this.minValue + parseInt((this.maxValue - this.minValue) * ((mouseX - leftX) / (rightX - leftX)), 10);
      }
      this.onValueChange(this.value);
    },
    removeScrollListener() {
      window.removeEventListener('touchmove', this.scrollListener);
      window.removeEventListener('mousemove', this.scrollListener);
      window.removeEventListener('mouseup', this.removeScrollListener);
      window.removeEventListener('touchend', this.removeScrollListener);
    },
    startScrolling(event) {
      if (this.disabled) {
        return;
      }
      const mouseX = event.clientX || event.touches[0].pageX;
      this.moveBall(mouseX);
      window.addEventListener('touchmove', this.scrollListener);
      window.addEventListener('mousemove', this.scrollListener);
      window.addEventListener('mouseup', this.removeScrollListener);
      window.addEventListener('touchend', this.removeScrollListener);
    }
  }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.wrapper {
  background: #FFFFFF;
  box-shadow: 0 0 10px 0 rgba(0,0,0,0.10);
  border-radius: 5px;
  padding: 17px 20px 0;
}
.title {
  display: block;
  text-align: center;
  font-weight: normal;
  font-size: 18px;
  line-height: 25px;
}
.scroll-section {
  position:relative;
  width: 100%;
  height: 21px;
  margin-top: 10px;
}
.light-bar {
  position: absolute;
  height: 5px;
  width: 100%;
  top: 5px;
  left: 0;
  border-radius: 2.5px;
}
.dark-bar {
  position: absolute;
  height: 5px;
  top: 5px;
  left: 0;
  border-radius: 2.5px;
}
.ball {
  position: absolute;
  width: 21px;
  height: 21px;
  top: -3px;
  transform: translateX(-50%);
  border: 3px solid #FFFFFF;
  border-radius: 21px;
  z-index: 2;
}
.description {
  display: block;
  margin-top: 7px;
  font-size: 13px;
  color: #42506B;
  letter-spacing: 0;
  text-align: center;
  line-height: 25px;
}
</style>
