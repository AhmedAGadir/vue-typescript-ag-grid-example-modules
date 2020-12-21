<template>
  <button @click="clicked">{{params.value}}</button>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";

@Component
export default class MyRenderer extends Vue {
  clicked() {
    const cellValue = (this as any).params.value;
    (this as any).params.context.componentParent.exec(cellValue);
  }
}
</script>

<style scoped lang="scss">
</style>
