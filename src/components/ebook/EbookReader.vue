<template>
  <div class="ebook-reader">
    <div id="read"></div>
  </div>
</template>

<script>
  import { mapGetters } from 'vuex'
  import Epub from 'epubjs'
  global.epub = Epub
  export default {
    computed: {
      ...mapGetters(['fileName'])
    },
    methods: {
      prevPage() {
        if (this.rendition) {
          this.rendition.prev()
        }
      },
      nextPage() {
        if (this.rendition) {
          this.rendition.next()
        }
      },
      showTitleAndMenu() {},
      initEpub: function () {
        const url = 'http://127.0.0.1:8081/epub/' + this.fileName + '.epub'
        this.book = new Epub(url)
        // 渲染对象
        this.rendition = this.book.renderTo('read', {
          width: window.innerWidth,
          height: window.innerHeight,
          method: 'default'// 微信的兼容性，可以在微信上正常使用
        })
        this.rendition.display()
        this.rendition.on('touchstart', event => {
          this.touchStartX = event.changedTouches[0].clientX
          this.touchStartTime = event.timeStamp
        })
        this.rendition.on('touchend', event => {
          const offsetX = event.changedTouches[0].clientX - this.touchStartX
          const time = event.timeStamp - this.touchStartTime
          if (time < 500 && offsetX > 40) {
            this.prevPage()
          } else if (time < 500 && offsetX < -40) {
            this.nextPage()
          } else {
            this.showTitleAndMenu()
          }
          event.preventDefault()
          event.stopPropagation()
        })
      }
    },
    mounted () {
      const fileName = this.$route.params.fileName.split('|').join('/')
      this.$store.dispatch('setFileName', fileName).then(() => {
          this.initEpub()
      })
    }
  }
</script>

<style lang="scss" rel="stylesheet/scss" scoped>
  @import "../../assets/styles/global";
</style>
