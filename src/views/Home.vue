<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <h1>Default</h1>
    <section class="inline-block">
      <FrostedGlass></FrostedGlass>
    </section>
  </div>
</template>

<script>
// @ is an alias to /src
import FrostedGlass from "@/components/FrostedGlass.vue";

export default {
  name: "Home",
  components: {
    FrostedGlass
  }
};
</script>

<style lang="stylus" scoped>
.inline-block {
  display: inline-block;
  width: 800px;
  height: 600px;
  border-radius: 1em;
  position relative
  background-color #F6F6F6
}
</style>