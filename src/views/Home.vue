<style lang="stylus" scoped>
.inline-block {
  display: inline-block;
  width: 800px;
  height: 600px;
  border-radius: 1em;
  position: relative;
  background-color: #F6F6F6;
}

.my-desktop {
  height: 600px;
}
</style>
<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <WindowManager class="my-desktop"></WindowManager>
  </div>
</template>

<script>
// @ is an alias to /src
import MoveableSection from "@/components/MoveableSection.vue";
import Window from "@/components/Window.vue";
import WindowManager from "@/components/WindowManager.vue";

export default {
  name: "Home",
  components: {
    MoveableSection,
    Window,
    WindowManager
  }
};
</script>

