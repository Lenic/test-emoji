<template>
  <div id="app">
    <div>{{text}}</div>
    <hr/>
    <div v-html="result"></div>
    <hr/>
    <div>{{original}}</div>
  </div>
</template>

<script>
const emojiRegex = require('emoji-regex')

const imgRegex = /<img\ssrc="[A-Za-z0-9/.-]*?"\sclass="row-text"\salt="(.*?)"\s\/>/gu

export default {
  name: 'App',
  data () {
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
  mounted () {
    const regex = emojiRegex()

    this.result = this.text.replace(regex, v => {
      const mapValue = this.mapper[v]
      if (mapValue) {
        return `<img src="/static/emoji/${this.mapper[v]}.png" class="row-text" alt="${v}" />`
      } else {
        return v
      }
    })

    console.log(this.result)

    this.original = this.result.replace(imgRegex, '$1')
  }
}
</script>

<style>
.row-text {
  /* width: 20px;
  height: 20px; */
}
</style>
