<template>
  <div class="__section__" :style="style" @mousedown="dragstart" @mouseup="dragend"></div>
</template>

<script lang="ts">
import Vue from "vue";

export default Vue.extend({
  name: "FrostedGlass",
  data() {
    return {
      position: {
        x: 0,
        y: 0
      },
      size: {
        w: 100,
        h: 100
      },
      draging: {
        status: false
      },
      style: {
        top: `${0}px`,
        height: `${100}px`,
        left: `${0}px`,
        width: `${100}px`
      }
    };
  },
  watch: {
    position: {
      handler() {
        this.style.top = `${this.position.y}px`;
        this.style.left = `${this.position.x}px`;
      },
      deep: true
    },
    size: {
      handler() {
        this.style.height = `${this.size.h}px`;
        this.style.width = `${this.size.w}px`;
      },
      deep: true
    }
  },
  methods: {
    dragstart(event: DragEvent) {
      // console.log(event);
      this.draging.status = true;
    },
    drag(event: DragEvent) {
      console.log(event.movementX);
      console.log(event.movementY);
    },
    mousemove(event: MouseEvent) {
      if (this.draging.status) {
        console.log(event.movementX);
        console.log(event.movementY);
        this.position.x += event.movementX;
        this.position.y += event.movementY;
      }
    },
    dragend(event: MouseEvent) {
      // console.log(event);
      this.draging.status = false;
    }
  },
  async mounted() {
    window.addEventListener("mousemove", this.mousemove);
    window.addEventListener("mouseup", this.dragend);
  },
  async beforeDestroy() {
    window.removeEventListener("mousemove", this.mousemove);
    window.addEventListener("mouseup", this.dragend);
  }
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="stylus">
.__section__ {
  position: absolute;
  background-color: green;
  border-radius: 1em;
}
</style>
