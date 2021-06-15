<template>
  <Layout>
    <div class="journal">
      <div class="container journal-container">

        <div class="journal-header">
          <h1 v-html="$page.post.title" class="journal-title" />
          <div class="journal-meta">
            <div class="journal-author">
              <span class="label">Author</span>
              <span class="author-name" v-text="$page.post.author.name" />
            </div>
            <div class="journal-date">
              <span class="label">Date</span>
              <div v-text="date($page.post.published_at, 'DD MMM , YYYY')"/>
            </div>
            <div class="journal-time">
              <span class="label">Time</span>
              <span>{{ $page.post.time }} min read</span>
            </div>
          </div>          
        </div>

        <div v-html="mdToHtml($page.post.content)"></div>

      </div>
    </div>
  </Layout>
</template>

<page-query>
query ($id: ID) {
  post: strapiJournal (id: $id) {
    title
    content
    published_at
    time
    author {
      name
    }
  }
}
</page-query>

<script>
import MarkdownIt from 'markdown-it'
import dayjs from 'dayjs'
const md = new MarkdownIt()

export default {
  metaInfo () {
    return {
      title: this.$page.post.title
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
.journal-container {
  max-width: 840px;
}
.journal-header {
  padding: 2rem 0 4rem 0;
}
.journal-title {
  font-size: 4rem;
  margin: 0 0 4rem 0;
  padding: 0;
}
.journal-meta {
  display: flex;
  flex-wrap: wrap;
  font-size: 0.8rem;
}
.journal-meta > div {
  margin-right: 4rem;
}
.journal-meta > div:last-of-type {
  margin: 0;
}
</style>