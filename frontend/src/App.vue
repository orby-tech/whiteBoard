<template>
  <div id="app">
    <Base />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import Base from "./components/Base.vue";

@Component({
  components: {
    Base
  }
})
export default class App extends Vue {}
</script>

<style lang="scss">
body{
  margin: 0;
  padding: 0;
}
</style>
