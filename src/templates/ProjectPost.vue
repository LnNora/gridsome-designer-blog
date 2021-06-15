<template>
  <Layout>
    <div class="project">

      <div class="container">

        <div class="project-header">
          <h1 class="project-title" v-html="$page.post.title" />
          <div class="project-info">

            <div class="categories-container">
              <div class="categories">
                <span class="label">Categories</span>
                <span 
                  class="category"
                  v-for="(category, index) in $page.post.categories" 
                  :key="index"
                  v-text="category.title"
                />
              </div>
            </div>

            <div class="year-container">
              <span class="label">Year</span>
              <div v-html="date($page.post.published_at, 'YYYY')"/>
            </div>
          </div>
        </div>

        <div v-html="mdToHtml($page.post.content)" class="content" />

      </div>

    </div>
  </Layout>
</template>

<page-query>
query ($id: ID) {
  post: strapiProject(id: $id) {
    title
    content
    published_at
    bgColor
    fgColor
    thumbnail {
      url
    }
    categories {
      title
    }
  }
}
</page-query>

<script>
import dayjs from 'dayjs'
import MarkdownIt from 'markdown-it'
const md = new MarkdownIt()
export default {
  metaInfo () {
    return {
      title: this.$page.post.title,
      bodyAttrs: {
        style: `background-color: ${this.$page.post.bgColor ? this.$page.post.bgColor : 'var(--color-base)'}; color: ${this.$page.post.fgColor ? this.$page.post.fgColor : 'var(--color-contrast)'}`
      }
    }
  },
  methods: {
    mdToHtml (markdown ) {
      return md.render(markdown)
    },
    date (value, format = 'YYYY-MM-DD HH:mm:ss'){
      return dayjs(value).format(format)
    }
  }
}
</script>

<style scoped>
.project-header {
  padding: 20vh 0 4rem 0;
}
.project-title {
  font-size: 4rem;
  margin: 0 0 4rem 0;
  padding: 0;
}
.project-info {
  display: flex;
  flex-wrap: wrap;
  font-size: 0.8rem;
}
.project-info > div {
  margin-right: 4rem;
}
.project-info > div:last-of-type {
  margin: 0;
}
.category:after {
  content: ', '
}
.category:last-of-type:after {
  content: '';
}
</style>