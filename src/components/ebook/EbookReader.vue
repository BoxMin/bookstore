<template>
  <div class="ebook-reader">
    <div id="read"></div>
  </div>
</template>

<script>
  // eslint-disable-next-line no-unused-vars
  import { mapGetters } from 'vuex'
  import Epub from 'epubjs'
  global.epub = Epub
  export default {
    computed: {
      ...mapGetters(['fileName'])
    },
    methods: {
      initEpub () {
        const url = 'http://127.0.0.1:8081/epub/' + this.fileName + '.epub'
        this.book = new Epub(url)
        // 渲染对象
        this.rendition = this.book.renderTo('read', {
          width: innerWidth,
          height: innerHeight,
          method: 'default'
        })
        this.rendition.display()
      }
    },
    mounted () {
      // eslint-disable-next-line no-unused-vars
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
