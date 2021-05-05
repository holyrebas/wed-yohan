<template>
  <div class="wedding-editor" ref="editor">
    <header class="editor-header">
        <a href="javascript:;" @click="skipAnimation()"></a>
        <a href="javascript:;" @click="skipAnimation()" class="minimum"></a>
        <a href="javascript:;" @click="skipAnimation()" class="maximum"></a>
        <span id="editor-header-button">&nbsp;</span>
        <b-tooltip ref="tooltip" target="editor-header-button" placement="right">Touch to skip</b-tooltip>
    </header>
    <!-- Date -->
    <p class="code" id="coda"><span>Last Login: {{ startDate }}</span></p>
    <!-- Code area -->
    <pre>
      <code v-html="highlightedCode"></code>
    </pre>
    <!-- Open invitation -->
    <div
      class="editor-open"
      v-if="(canStart || hasClosed) && !canOpen"
      @click="canOpen = true"
    >
      <b-icon icon="envelope-open" style="width: 25px; height: 25px; color: #BBB" aria-hidden="true"></b-icon>
    </div>
    <Executions :canExecute="canExecute" @onUpdating="scrollToBottom" @onFinish="canOpen = true"/>
    <invitation :canOpen="canOpen" @onClose="canOpen = false, hasClosed = true"/>
  </div>
</template>

<script>
  import Prism from 'prismjs'
  import 'prismjs/themes/prism-okaidia.css'
  import '../utils/raf'
  import data from '../mock/data'

  import Executions from './Executions'
  import Invitation from './Invitation'

  export default {
    name: 'Editor',
    components: { Executions, Invitation },
    data() {
      return {
        startDate: '',
        code: data.code,
        currentCode: '',
        isCursorVisible: 1,
        canExecute: false,
        canOpen: false,
        wish: '',
        hasClosed: false,
        canStart: false,
        isSkipped: false,
      }
    },
    created() {
      this.startDate = (new Date()).toDateString()
      this.progressivelyTyping()
    },
    updated() {

      this.scrollToBottom()
    },
    computed: {
      highlightedCode () {
        const code = Prism.highlight(
            this.currentCode,
            Prism.languages.javascript
          )
        const codeWithCursor = `${code}<span style="opacity:${this.isCursorVisible}"> ▍</span>`
        return codeWithCursor
      }
    },
    methods: {
      scrollToBottom() {
        // Scroll down to bottom end
        this.$refs.editor.scrollTop = 100000
      },
      // Typing
      progressivelyTyping() {
        return new Promise((resolve) => {
          let count = 0, typingCount = 0, typing
          // Writing functions
          let step = () => {
            let randomNumber = Math.round(Math.random() * 6)
            // Random typing
            if(count % 2 === 0 && randomNumber % 4 === 0){
              this.currentCode = this.code.substring(0, typingCount)
              typingCount++
            }
            // 24 frames
            if(count % 24 === 0){
              this.isCursorVisible = this.isCursorVisible === 0 ? 1 : 0
            }
            count++
            if (this.isSkipped) {
              typing = requestAnimationFrame(step)
            }
            if (typingCount <= this.code.length) {
              typing = requestAnimationFrame(step)
            } else {
              resolve()
              this.canExecute = true
              cancelAnimationFrame(typing)
            }
          }
          typing = requestAnimationFrame(step)
          setTimeout(() => {
            if (!this.isSkipped) {
              this.$refs.tooltip.$emit('open')
              setTimeout(() => {
                this.$refs.tooltip.$emit('close')
              }, 2000);
            }
          }, 10000);
        })
      },
      skipAnimation() {
        this.$refs.tooltip.$emit('close')
        this.isSkipped = true
      },
    }
  }
</script>

<style lang="less">
.wedding-editor{
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  padding: 15px;
  overflow-x: hidden;
  overflow-y: auto;
  z-index: 1;
  transform-origin: 0 0;
  -webkit-transform-origin: 0 0;
  transition: all 1.6s cubic-bezier(0.4, 0, 1, 1);
  -webkit-transition: all 1.6s cubic-bezier(0.4, 0, 1, 1);
  .editor-header{
    position: sticky;
    top: 0;
    left: 0;
    width: 100%;
    margin-bottom: 20px;
    overflow: hidden;
    background: #2B2B48;
    z-index: 3;
    >a{
      float: left;
      display: block;
      width: 16px;
      height: 16px;
      margin-right: 5px;
      border-radius: 8px;
      background: #FC615D;
      &.minimum{
        background: #FDBC40;
      }
      &.maximum{
        background: #34C84A;
      }
    }
    #editor-header-button {
      margin-left: 1px;
    }
  }
  p.code{
    margin: 0;
    color: #BBB;
    line-height: 1.2;
    font-family: 'Fira Code', 'Nanum Gothic Coding', monospace !important;
    font-weight: 400 !important;
    font-size: 14px!important;
  }
  pre{
    margin: 0;
    white-space: pre-wrap;
    code{
      white-space: pre-wrap;
      word-break: break-all;
      font-size: 14px!important;
      margin: 0;
      color: #BBB;
      line-height: 1.2;
      font-family: 'Fira Code', 'Nanum Gothic Coding', monospace !important;
      font-weight: 400 !important;
      background: transparent;
    }
  }
  .editor-open{
    position: absolute;
    right: 25px;
    bottom: 25px;
    z-index: 1000;
  }
  .editor-open:hover {
    cursor: pointer;
  }
}
</style>