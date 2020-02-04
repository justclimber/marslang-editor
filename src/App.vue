<template>
  <div id="app">
    <CodeEditor />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import CodeEditor from "./components/CodeEditor.vue";

@Component({
  components: {
    CodeEditor
  }
})
export default class App extends Vue {}
</script>

<style lang="stylus">
body
  margin 0
  padding 0
#app
  font-family 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  color #2c3e50
  margin 0
  padding 0
</style>
