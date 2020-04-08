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
    @mousedown="dragstart($event,'move')"
  >
    <!-- resize_x_md_top  -->
    <div @mousedown.stop="dragstart($event,'resize_x_md_top')" class="handle x-md top"></div>
    <!-- resize_x_md_bottom -->
    <div @mousedown.stop="dragstart($event,'resize_x_md_bottom')" class="handle x-md bottom"></div>
    <!-- resize_y_md_left -->
    <div @mousedown.stop="dragstart($event,'resize_y_md_left')" class="handle y-md left"></div>
    <!-- resize_y_md_right -->
    <div @mousedown.stop="dragstart($event,'resize_y_md_right')" class="handle y-md right"></div>

    <!-- resize_left_top -->
    <div @mousedown.stop="dragstart($event,'resize_left_top')" class="handle left top square"></div>
    <!-- resize_right_top -->
    <div @mousedown.stop="dragstart($event,'resize_right_top')" class="handle right top square"></div>
    <!-- resize_right_bottom -->
    <div
      @mousedown.stop="dragstart($event,'resize_right_bottom')"
      class="handle right bottom square"
    ></div>
    <!-- resize_left_bottom -->
    <div @mousedown.stop="dragstart($event,'resize_left_bottom')" class="handle left bottom square"></div>
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
        status: false,
        type: ""
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
    dragstart(event: MouseEvent, type: string) {
      this.draging.status = true;
      this.draging.type = type;
    },
    drag(event: MouseEvent) {
      switch (this.draging.type) {
        case "move":
          this.position.x += event.movementX;
          this.position.y += event.movementY;
          break;
        case "resize_x_md_top":
        case "resize_x_md_bottom":
        case "resize_y_md_left":
        case "resize_y_md_right":

        case "resize_left_top":
        case "resize_right_top":
        case "resize_right_bottom":
        case "resize_left_bottom":

        default:
          console.error("undefined drag op");
          break;
      }
    },
    mousemove(event: MouseEvent) {
      if (this.draging.status) {
        this.drag(event);
      }
    },
    dragend(event: MouseEvent) {
      this.draging.status = false;
      this.draging.type = "";
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

