<template>
  <div class="__section__"></div>
</template>

<script lang="ts">
import Vue from "vue";
export default Vue.extend({
  name: "FrostedGlass"
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="stylus">
.__section__ {
  position: absolute;
  top: 100px;
  bottom: 100px;
  left: 100px;
  right: 100px;
  background-color: green;
  border-radius: 1em;
}
</style>
