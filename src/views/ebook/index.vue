<template>
  <div class="ebook">
    <ebook-reader></ebook-reader>
    <ebook-title></ebook-title>
    <ebook-menu></ebook-menu>
  </div>
</template>

<script>
  import EbookReader from '../../components/ebook/EbookReader'
  import EbookTitle from '../../components/ebook/EbookTitle'
  import EbookMenu from '../../components/ebook/EbookMenu'
  export default {
    components: {
      EbookMenu,
      EbookTitle,
      EbookReader
    }
  }
</script>

<style lang="scss" rel="stylesheet/scss" scoped>
  @import "../../assets/styles/global";
</style>
