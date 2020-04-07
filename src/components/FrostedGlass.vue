<template>
  <div class="__frosted-glass-wrap-box__">
    <div class="window-glass" :style="style_window_glass"></div>
    <slot>
    </slot>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
export default Vue.extend({
  name: "FrostedGlass",
  props: {
    filter: {
      type: String,
      default: `blur(36px)`
    },
    backgroundImage: {
      type: String,
      required: true
    }
  },
  computed: {
    style_window_glass() {
      return {
        filter: this.filter,
        "background-image": this.backgroundImage
      };
    }
  }
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="stylus">
.__frosted-glass-wrap-box__ {
  overflow: hidden;
  position: relative;
  border-radius: 1em;

  .window-glass {
    // filter: blur(36px);
    // background: url('./assets/bg-1.png');
    position: absolute;
    background-position: center;
    background-size: cover;
    z-index: -1;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
  }

  .content, .window-glass {
    height: 100%;
    width: 100%;
  }

  .content {
  }
}
</style>
