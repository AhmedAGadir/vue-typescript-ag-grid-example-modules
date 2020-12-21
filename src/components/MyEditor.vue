<template>
  <input type="text" v-model="value" ref="val" />
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";

@Component
export default class MyEditor extends Vue {
  private value = "";
  public getValue(): string {
    return this.value;
  }
  public afterGuiAttached(): void {
    (this.$refs.val as any).focus();
    (this.$refs.val as any).select();
  }
  created() {
    this.value = (this as any).params.value;
  }
}
</script>

<style scoped lang="scss">
</style>
