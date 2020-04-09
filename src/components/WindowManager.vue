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
    <Window>
      <template #header>标题1</template>
      <iframe class="content" src="https://www.baidu.com" frameborder="0"></iframe>
    </Window>
    <Window>
      <template #header>标题2</template>
      <iframe class="content" src="https://www.baidu.com" frameborder="0"></iframe>
    </Window>
    <Window>
      <template #header>标题3</template>
      <iframe class="content" src="https://www.baidu.com" frameborder="0"></iframe>
    </Window>
  </div>
</template>

<script>
import Window from "@/components/Window.vue";

export default {
  name: "WindowManager",
  components: {
    Window
  }
};
</script>

