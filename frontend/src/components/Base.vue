<template>
  <div>
      <Layers />

  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";
import Layers from '@/components/Layers.vue'
@Component({
    components: {
        Layers
    }
})
export default class Base extends Vue {
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
</style>
