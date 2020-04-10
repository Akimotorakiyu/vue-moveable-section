<style lang="stylus" scoped>
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
    // padding: 0 2px 2px 2px;
  }
}
</style>
<template>
  <MoveableSection>
    <div class="window">
      <header class="my-header">
        <slot name="header"></slot>
      </header>
      <main class="my-main" @mousedown.stop>
        <slot></slot>
      </main>
    </div>
  </MoveableSection>
</template>

<script>
// @ is an alias to /src
import MoveableSection from "@/components/MoveableSection.vue";

import Vue from "vue";

export default Vue.extend({
  name: "Window",
  components: {
    MoveableSection
  }
});
</script>

