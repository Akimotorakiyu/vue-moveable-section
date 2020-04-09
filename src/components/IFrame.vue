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
}
</style>
<template>
  <section class="__container__">
    <iframe frameborder="0" :src="src"></iframe>
    <div class="modal"></div>
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
    }
  },
  async mounted() {
    window.addEventListener("draging", this.dragstart);
    window.addEventListener("mouseup", this.dragend);
  },
  async beforeDestroy() {
    window.removeEventListener("draging", this.dragstart);
    window.addEventListener("mouseup", this.dragend);
  }
});
</script>