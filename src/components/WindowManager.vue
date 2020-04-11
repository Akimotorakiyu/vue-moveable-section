<style lang="stylus" scoped>
.window-manager {
  border-radius: 1em;
  position: relative;
  background-color: #F6F6F6;
}

.window {
  display: flex;
  height: 100%;
  flex-direction: column;

  .my-header {
    cursor: default;
    user-select: none;
    background-color: black;
    color: white;
  }

  .my-main {
    flex: 1;
  }

  .content {
    width: 100%;
    height: 100%;
  }
}
</style>
<template>
  <div class="window-manager">
    <el-button type="primary" circle @click="newWindow">
      <i class="el-icon-circle-plus-outline"></i>
    </el-button>

    <template v-for="(win,index) in windows">
      <Window :key="win._id">
        <template #header>
          {{win.title}}
          <i class="el-icon-circle-close" @click="close(index)"></i>
        </template>
        <IFrame class="content" :src="win.src"></IFrame>
      </Window>
    </template>
  </div>
</template>

<script lang="ts">
import Window from "@/components/Window.vue";
import IFrame from "@/components/IFrame.vue";
import { nanoid } from "nanoid";
import Vue from "vue";

interface WindowsDesc {
  _id: string;

  title: string;
  src: string;
}
let count = 0;
export default Vue.extend({
  name: "WindowManager",
  props: {
    windows: {
      default: () =>
        [{ title: "标题", src: "https://www.baidu.com" }] as WindowsDesc[]
    }
  },
  components: {
    Window,
    IFrame
  },
  methods: {
    newWindow(e: MouseEvent) {
      this.windows.push({
        _id: nanoid(),
        title: "标题" + Math.random().toFixed(2),
        src: "https://www.baidu.com"
      });
    },
    close(index: number) {
      console.log(index);
      this.windows.splice(index, 1);
    }
  }
});
</script>

