<style lang="stylus" scoped>
.__container__ {
  position: relative;

  .modal {
    position: absolute;
    left: 0;
    top: 0;
    bottom: 0;
    right: 0;
  }

  .frame {
    width: 100%;
    height: 100%;
  }
}
</style>
<template>
  <section class="__container__">
    <iframe class="frame" frameborder="0" :src="src"></iframe>
    <div class="modal" v-show="draging"></div>
  </section>
</template>

<script lang="ts">
import Vue from "vue";
export default Vue.extend({
  props: {
    src: {
      type: String,
      default: ""
    }
  },
  data() {
    return { draging: true };
  },
  methods: {
    dragstart() {
      this.draging = true;
    },
    dragend() {
      this.draging = false;
    },
    isOutIframe(event: MouseEvent) {
      if ((event.target as HTMLElement).tagName === "IFRAME") {
        this.dragend();
      }
    }
  },
  async mounted() {
    window.addEventListener("movestart", this.dragstart);
    window.addEventListener("movend", this.dragend);
  },
  async beforeDestroy() {
    window.removeEventListener("movestart", this.dragstart);
    window.removeEventListener("movend", this.dragend);
  }
});
</script>