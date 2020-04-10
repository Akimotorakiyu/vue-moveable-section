<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="stylus">
.__section__ {
  position: absolute;
  background-color: #ffffff;
  border-radius: 4px;
  overflow: hidden;

  // transition: all 0.032s ease-in-out;

  // cursor: grab;
  &.dragging {
    // cursor: grabbing;
  }
}

.handle {
  position: absolute;

  // background-color: transparent;
  // background-color: red;
  &.square {
    height: 4px;
    width: 4px;
    // background-color: yellow;
  }

  &.x-md {
    height: 4px;
    right: 0;
    cursor: ns-resize;
    left: 0;
  }

  &.y-md {
    top: 0;
    bottom: 0;
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
    <slot></slot>
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

interface Zone {
  x: number;
  y: number;
  w: number;
  h: number;
}

function controlGrid(x: number, grid: number) {
  return grid ? x - (x % grid) : x;
}

export default Vue.extend({
  name: "MoveableSection",
  props: {
    grid: {
      type: Number,
      default: 4
    },
    zone: {
      default: () => ({ x: 0, y: 0, w: 400, h: 300 } as Zone)
    },
    useTransform: {
      type: Boolean,
      default: true
    }
  },
  data() {
    return {
      draging: {
        status: false,
        type: "",
        initStatus: {
          pageX: 0,
          pageY: 0,
          x: 0,
          y: 0,
          w: 0,
          h: 0
        }
      },
      style: {
        top: `${0}px`,
        left: `${0}px`,
        height: `${this.zone.h}px`,
        width: `${this.zone.w}px`,
        transform: `translate(${0}px,${0}px)`
      }
    };
  },
  watch: {
    zone: {
      handler(newValue: Zone, oldValue: Zone) {
        // console.log(
        //   JSON.stringify(newValue, undefined, 4),
        //   JSON.stringify(oldValue, undefined, 4)
        // );

        if (this.useTransform) {
          this.style.transform = `translate(${this.zone.x}px,${this.zone.y}px)`;
        } else {
          this.style.left = `${this.zone.x}px`;
          this.style.top = `${this.zone.y}px`;
        }

        this.style.width = `${this.zone.w}px`;
        this.style.height = `${this.zone.h}px`;
      },
      deep: true,
      immediate: true
    }
  },
  methods: {
    dragstart(event: MouseEvent, type: string) {
      this.draging.status = true;
      this.draging.type = type;
      this.draging.initStatus.pageX = event.pageX;
      this.draging.initStatus.pageY = event.pageY;

      this.draging.initStatus.x = this.zone.x;
      this.draging.initStatus.y = this.zone.y;

      this.draging.initStatus.w = this.zone.w;
      this.draging.initStatus.h = this.zone.h;

      window.dispatchEvent(new UIEvent("movestart"));
    },
    drag(event: MouseEvent) {
      const movementX = event.pageX - this.draging.initStatus.pageX;
      const movementY = event.pageY - this.draging.initStatus.pageY;

      // console.log("xxxx", this.draging.type);

      let { x, y, w, h } = this.draging.initStatus;

      switch (this.draging.type) {
        case "move":
          x = this.draging.initStatus.x + movementX;
          y = this.draging.initStatus.y + movementY;
          break;
        case "resize_x_md_top":
          y = this.draging.initStatus.y + movementY;
          h = this.draging.initStatus.h - movementY;
          break;
        case "resize_x_md_bottom":
          h = this.draging.initStatus.h + movementY;
          break;
        case "resize_y_md_left":
          x = this.draging.initStatus.x + movementX;
          w = this.draging.initStatus.w - movementX;
          break;
        case "resize_y_md_right":
          w = this.draging.initStatus.w + movementX;
          break;

        case "resize_left_top":
          y = this.draging.initStatus.y + movementY;
          h = this.draging.initStatus.h - movementY;

          x = this.draging.initStatus.x + movementX;
          w = this.draging.initStatus.w - movementX;
          break;
        case "resize_right_top":
          w = this.draging.initStatus.w + movementX;

          y = this.draging.initStatus.y + movementY;
          h = this.draging.initStatus.h - movementY;

          break;
        case "resize_right_bottom":
          w = this.draging.initStatus.w + movementX;

          h = this.draging.initStatus.h + movementY;
          break;
        case "resize_left_bottom":
          x = this.draging.initStatus.x + movementX;
          w = this.draging.initStatus.w - movementX;

          h = this.draging.initStatus.h + movementY;
          break;

        default:
          console.error("undefined drag op", this.draging.type);
          break;
      }
      const grid = this.grid;
      // this.zone.x = x;
      // this.zone.y = y;
      // this.zone.w = w;
      // this.zone.h = h;
      this.zone.x = controlGrid(x, grid);
      this.zone.y = controlGrid(y, grid);
      this.zone.w = controlGrid(w, grid);
      this.zone.h = controlGrid(h, grid);
    },
    mousemove(event: MouseEvent) {
      if (this.draging.status) {
        this.drag(event);
      }
    },
    dragend(event: MouseEvent) {
      this.draging.status = false;
      this.draging.type = "";

      window.dispatchEvent(new UIEvent("movend"));
    },
    isOutIframe(event: MouseEvent) {
      if ((event.target as HTMLElement).tagName === "IFRAME") {
        this.dragend(event);
      }
    }
  },
  async mounted() {
    window.addEventListener("mousemove", this.mousemove);
    window.addEventListener("mouseup", this.dragend);
    window.addEventListener("mouseout", this.isOutIframe);
  },
  async beforeDestroy() {
    window.removeEventListener("mousemove", this.mousemove);
    window.removeEventListener("mouseup", this.dragend);
    window.removeEventListener("mouseout", this.isOutIframe);
  }
});
</script>

