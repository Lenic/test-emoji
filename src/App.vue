<template>
  <div id="app">
    <div>{{text}}</div>
    <hr/>
    <div v-html="result"></div>
    <hr/>
    <div>{{original}}</div>
    <hr/>
    <button @click="addEmojiHandler('😠')">
      <img alt="😠"
           class="row-text"
           src="/static/emoji/angry.png" />
    </button>
    <div class="box"
         ref="content"
         @paste="pasteHandler"
         contentEditable="true">
    </div>
  </div>
</template>

<script>
const emojiRegex = require('emoji-regex')

const imgRegex = /<img\ssrc="[A-Za-z0-9/.-]*?"\sclass="row-text"\salt="(.*?)"\s\/>/gu

export default {
  name: 'App',
  data() {
    return {
      result: '',
      original: '',
      text: '😠等待🍎纷😊送纷👶跟单发送🐌访港👧扽🐷色对的',
      mapper: {
        '😠': 'angry',
        '🍎': 'apple',
        '👶': 'baby',
        '🐌': 'snail',
        '👧': 'girl'
      }
    }
  },
  mounted() {
    const regex = emojiRegex()

    this.result = this.text.replace(regex, v => {
      const mapValue = this.mapper[v]
      if (mapValue) {
        return `<img src="/static/emoji/${this.mapper[v]}.png" class="row-text" alt="${v}" />`
      } else {
        return v
      }
    })

    this.original = this.result.replace(imgRegex, '$1')
  },
  methods: {
    addEmojiHandler(v) {
      const html = `<img src="/static/emoji/${this.mapper[v]}.png" class="row-text" alt="${v}" />`

      this.$refs.content.innerHTML += html
    },
    pasteHandler(e) {
      debugger
    }
  }
}
</script>

<style>
.row-text {
  /* width: 20px;
  height: 20px; */
}

.box {
  width: 400px;
  min-height: 100px;
  max-height: 300px;
  height: auto;
  outline: none;
  border: 1px solid #f00;
  overflow-y: auto;
}
</style>
