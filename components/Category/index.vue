<template>
  <div class="categories-wrapper" v-if="isCategories">
    <note-abstract 
      :data="pages"
      :currentPage="currentPage"
      @currentTag="getCurrentTag"></note-abstract>
    
    <pagation 
      :data="pages"
      :currentPage="currentPage"
      @getCurrentPage="getCurrentPage"></pagation>
  </div>
</template>

<script>
import NoteAbstract from '../NoteAbstract/'
import Pagation from '../Pagation/'

export default {
  data () {
    return {
      pages: [],
      currentPage: 1
    }
  },

  computed: {
    isCategories () {
      let isCategories = this.$page.frontmatter.isCategories
      if (isCategories) {
        this.getPagesByCategories()
      } else {
        this.pages = []
      }
      
      return isCategories
    }
  },
  updated () {
    this.currentPage = this.$page.currentPage
  },
  methods: {
    // 根据分类获取页面数据
    getPagesByCategories () {
      let pages = this.$site.pages,
          title = this.$page.title
      pages = pages.filter(item => {
        return item.frontmatter.categories == title
      })
      // reverse()是为了按时间最近排序排序
      this.pages = pages.length == 0 ? [] : pages.reverse()
    },
    getCurrentTag (tag) {
      this.$emit('currentTag', tag)
    },
    getCurrentPage (page) {
      this.currentPage = page
      this.$page.currentPage = page
    }
  },
  components: {
    NoteAbstract,
    Pagation
  }
}
</script>

<style lang="stylus">
@import '../../styles/config.styl'
@require '../../styles/wrapper.styl'

.categories-wrapper
  max-width: 740px;
  margin: 0 auto;
  padding: 0 2.5rem; 

@media (max-width: $MQMobile)
  .categories-wrapper
    padding: 0 1rem;
  .page-edit
    .edit-link
      margin-bottom .5rem
    .last-updated
      font-size .8em
      float none
      text-align left

</style>
