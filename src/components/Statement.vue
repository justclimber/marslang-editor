<template>
  <div class="statement">
    <pre><code class="content" v-html="text" /></pre>
  </div>
</template>

<script lang="ts">
import { Component, Vue, Prop } from "vue-property-decorator";

@Component
export default class Statement extends Vue {
  @Prop(String) text: string | undefined;
}
</script>

<style lang="stylus"></style>
