<template>
  <div class="code-editor-container">
    <div class="txt-wrapper">
      <textarea
        class="txt original"
        v-model="sourceCode"
        spellcheck="false"
        ref="txt"
        @input="onTxtInput"
      />
    </div>
    <pre><code class="txt visualizer" v-html="sourceCodeHighlighted" /></pre>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";

function injectString(source: string, inject: string, pos: number): string {
  return (
    source.substring(0, pos) + inject + source.substring(pos, source.length)
  );
}

@Component
export default class CodeEditor extends Vue {
  $refs!: { txt: HTMLTextAreaElement };
  sourceCode: string = "some text";
  onTxtInput(event: any) {
    if (event.data === "{") {
      this.pairBracketsWithNewLineAndIndent();
    }
  }

  pairBracketsWithNewLineAndIndent(): void {
    const textToInsert = "\n   \n}";
    const pos = this.$refs.txt.selectionStart;
    this.sourceCode = injectString(this.sourceCode, textToInsert, pos);
    this.$nextTick(() => {
      this.gotoPos(pos + 4);
    });
  }

  gotoPos(pos: number): void {
    this.$refs.txt.selectionStart = pos;
    this.$refs.txt.selectionEnd = pos;
  }

  get sourceCodeHighlighted(): string {
    return this.sourceCode.replace(/var/g, "<span class='var'>var</span>");
  }
}
</script>

<style lang="stylus">
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
  top 0
  left 0
  z-index -1
.var
  color #a696ff
</style>
