<template>
  <div class="code-editor-container">
    <div>
      <button @click="toggleTxt">sdfsd</button>
    </div>
    <div class="txt-wrapper">
      <textarea
        class="txt original"
        v-model="sourceCode"
        spellcheck="false"
        ref="txt"
        @keyup="onTxtInput"
      />
    </div>
    <pre>
      <div class="txt visualizer" v-html="sourceCodeWithBr"></div>
    </pre>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";

declare global {
  interface HTMLTextAreaElement {
    insertAtCaret(text: string): void;
  }
}

HTMLTextAreaElement.prototype.insertAtCaret = function(text: string) {
  text = text || "";
  if (this.selectionStart || this.selectionStart === 0) {
    // Others
    const startPos = this.selectionStart;
    const endPos = this.selectionEnd;
    this.value =
      this.value.substring(0, startPos) +
      text +
      this.value.substring(endPos, this.value.length);
    this.selectionStart = startPos + text.length;
    this.selectionEnd = startPos + text.length;
  } else {
    this.value += text;
  }
};

@Component
export default class CodeEditor extends Vue {
  $refs!: { txt: HTMLTextAreaElement };
  sourceCode: string = "some text";
  onTxtInput(event: any) {
    if (event.key === "{") {
      this.$refs.txt.insertAtCaret("\n\n}");
    }
    console.log(event);
  }
  toggleTxt() {}

  get sourceCodeWithBr(): string {
    return this.sourceCodeHighlighted.replace(/\n/g, "<br/>");
  }
  get sourceInLines(): string[] {
    return this.sourceCode.split("\n");
  }
  get sourceCodeHighlighted(): string {
    return this.sourceCode.replace(/var/g, "<span class='var'>var</span>");
  }
}
</script>

<style lang="stylus">
.code-editor-container
  /*display flex*/

.txt
  width 500px
  height 500px
  border 1px solid grey
  margin 0
  padding 0
  font-size 18px
  font-weight normal
  font-family monospace
  color black
  overflow auto
  resize: none
  line-height 18px

.original
  z-index: 1
  opacity 0.1

.visualizer
  position absolute
  top 20px
  left 0
  z-index -1
.var
  color #a696ff
</style>
