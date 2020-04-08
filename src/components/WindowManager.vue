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
}
</style>
<template>
  <div class="window-manager">
    <Window>
      <template #header>标题1</template>
      内容1
    </Window>
    <Window>
      <template #header>标题2</template>
      内容2
    </Window>
    <Window>
      <template #header>标题3</template>
      内容3
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

