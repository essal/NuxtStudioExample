<template>
    <div ref="parallax" :class="['parallax']">
      <div ref="inner" class="inner" :style="{ height: imageHeight + 'px' }">
        <nuxt-picture :src="src" :img-attrs="{ class: 'image object-cover' }"/>
      </div>
    </div>
  </template>
  <script>
  export default {
    inject: {
      reduceMotion: {
        default: false,
      },
    },
    props: {
      src: {
        type: String,
        required: true,
      },
    },
    data: () => ({
      lastOffset: 0,
      intensity: 0.33,
      isRendering: false,
      rect: null,
      imageHeight: 0,
    }),
    computed: {
      ParallaxContainer() {
        return this.$refs.parallax;
      },
      InnerContainer() {
        return this.$refs.inner;
      },
      vh() {
        return window.innerHeight;
      },
    },
    watch: {
      reduceMotion(newVal) {
        // console.log({computed:newVal});
        if (!newVal) {
          this.init();
        }
      },
    },
  
    mounted() {
      this.init();
    },
    methods: {
      getScrollParent(node) {
        if (node == null) {
          return null;
        }
  
        const overflowY = window.getComputedStyle(node).overflowY;
        const isScrollable = overflowY !== "visible" && overflowY !== "hidden";
  
        if (isScrollable && node.scrollHeight > node.clientHeight) {
          return node;
        }
  
        if (document.body === node) {
          return node;
        }
  
        return this.getScrollParent(node.parentNode);
      },
      setHeight() {
        this.imageHeight =
          this.ParallaxContainer.offsetHeight * (1 + this.intensity); // + (this.vh / 8);
      },
  
      init() {
        this.isRendering = true;
        this.setPosition();
      },
      async setPosition() {
        // console.log({ test: this.reduceMotion });
        const y = this.ParallaxContainer.getBoundingClientRect().y;
        //const { y } = await asyncBounds(this.ParallaxContainer);
        // const test = this.getScrollParent(this.ParallaxContainer).offsetHeight / 2;
  
        const topOffset = y - window.innerHeight / 2;
  
        let offset = topOffset * (this.intensity * -1) - this.imageHeight / 4; // - ((this.imageHeight - this.ParallaxContainer.offsetHeight) / 4);
  
        if (this.reduceMotion) {
          offset = 0;
        }
  
        if (this.lastOffset !== offset) {
          this.InnerContainer.style.transform = `translate3d(0,${offset}px, 0)`;
          this.lastOffset = offset;
          this.setHeight();
        }
  
        if (this.isRendering && !this.reduceMotion) {
          requestAnimationFrame(this.setPosition);
        }
      },
    },
  };
  </script>
  <style scoped lang="scss">
  .parallax {
    height: 100%;
    position: relative;
    overflow: hidden;
    width: 100%;
    display: flex;
    .show {
      position: absolute;
      z-index: 1000000;
      font-size: 3em;
    }
  
    .inner {
      width: 100%;
      top: 0;
      left: 0;
  
      //height: calc(100vh - ((100vh - 100%) / 4));
  
      min-height: 100%;
      position: relative;
      display: flex;
      will-change: transform;
      background-size: cover;
      background-position: center;
      background-repeat: no-repeat;
      align-items: center;
  
      :deep(.image) {
        width: 100%;
        height: 100%;
        position: absolute;
        top: 0;
        left: 0;
      }
    }
  }
  </style>
  