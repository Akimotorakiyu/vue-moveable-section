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
    <template v-for="(win,index) in windows">
      <Window :key="index">
        <template #header>{{win.title}}</template>
        <IFrame class="content" :src="win.src"></IFrame>
      </Window>
    </template>
  </div>
</template>

<script lang="ts">
import Window from "@/components/Window.vue";
import IFrame from "@/components/IFrame.vue";

import Vue from "vue";

export default Vue.extend({
  name: "WindowManager",
  props: {
    windows: {
      default: () => [
        { title: "标题1", src: "https://www.baidu.com" },
        { title: "标题2", src: "https://www.baidu.com" },
        { title: "标题3", src: "https://www.baidu.com" }
      ]
    }
  },
  components: {
    Window,
    IFrame
  },
  methods: {
    test(e: MouseEvent) {
      console.log(e);
    }
  }
});
</script>

