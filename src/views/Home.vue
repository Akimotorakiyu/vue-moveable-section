<style lang="stylus" scoped>
.inline-block {
  display: inline-block;
  width: 800px;
  height: 600px;
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
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <h1>Default</h1>
    <section class="inline-block">
      <MoveableSection>
        <div class="window">
          <header class="my-header">hello world</header>
          <main class="my-main" @mousedown.stop>111</main>
        </div>
      </MoveableSection>
    </section>
  </div>
</template>

<script>
// @ is an alias to /src
import MoveableSection from "@/components/MoveableSection.vue";

export default {
  name: "Home",
  components: {
    MoveableSection
  }
};
</script>

