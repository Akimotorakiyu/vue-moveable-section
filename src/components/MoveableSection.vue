<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="stylus">
.__section__ {
  position: absolute;
  background-color: green;
  border-radius: 1em;
}

.handle {
  position: absolute;
  width: 4px;
  height: 4px;
  background-color: red;

  &.x-md {
    left: 50%;
  }

  &.y-md {
    top: 50%;
  }

  &.left {
    left: 0px;
  }

  &.right {
    right: 0px;
  }

  &.top {
    top: 0px;
  }

  &.bottom {
    bottom: 0px;
  }
}
</style>

<template>
  <div ref="section" class="__section__" :style="style" @mousedown="dragstart">
    <div class="handle x-md top"></div>
    <div class="handle x-md bottom"></div>
    <div class="handle y-md left"></div>
    <div class="handle y-md right"></div>
    <div class="handle left top"></div>
    <div class="handle right top"></div>
    <div class="handle right bottom"></div>
    <div class="handle left bottom"></div>
  </div>
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

