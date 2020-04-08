<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="stylus">
.__section__ {
  position: absolute;
  background-color: green;
  cursor: grab;

  &.dragging {
    cursor: grabbing;
  }
}

.handle {
  position: absolute;

  // background-color: red;
  &.square {
    height: 4px;
    width: 4px;
    // background-color: yellow;
  }

  &.x-square {
    height: 4px;
    width: 100%;
  }

  &.y-square {
    height: 100%;
    width: 4px;
  }

  &.x-md {
    height: 4px;
    width: 100%;
    cursor: ns-resize;
  }

  &.y-md {
    height: 100%;
    width: 4px;
    cursor: ew-resize;
  }

  &.left {
    left: 0px;

    &.bottom {
      cursor: nesw-resize;
    }

    &.top {
      cursor: nwse-resize;
    }
  }

  &.right {
    right: 0px;

    &.bottom {
      cursor: nwse-resize;
    }

    &.top {
      cursor: nesw-resize;
    }
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
  <div
    ref="section"
    :class="['__section__',draging.status?'dragging':'']"
    :style="style"
    @mousedown="dragstart"
  >
    <div @mousedown.stop="resize" class="handle x-md top"></div>
    <div @mousedown.stop="resize" class="handle x-md bottom"></div>
    <div @mousedown.stop="resize" class="handle y-md left"></div>
    <div @mousedown.stop="resize" class="handle y-md right"></div>
    <div @mousedown.stop="resize" class="handle left top square"></div>
    <div @mousedown.stop="resize" class="handle right top square"></div>
    <div @mousedown.stop="resize" class="handle right bottom square"></div>
    <div @mousedown.stop="resize" class="handle left bottom square"></div>
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
    resize(event: MouseEvent) {
      // event.cancelBubble=true
      console.log("xxxx");
    },
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

