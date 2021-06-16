<template>
  <Layout>
    <div class="container">
      <Hero :basicInfo="$page.basicInfo.edges[0].node"/>
      <ProjectsGrid :projects="$page.projects.edges" />
    </div>
    <LatestJournals :journals="$page.journals.edges" />
  </Layout>
</template>

<page-query>
query Posts {
	projects: allStrapiProject {
    edges {
      node {
        id
        updated_at
        title
        categories {
          title
        }
        thumbnail {
          url
        }
      }
    }
  }
  basicInfo: allStrapiGeneral {
    edges {
      node {
        title
        subtitle
      }
    }
  }
  journals: allStrapiJournal(perPage: 4) {
    edges {
      node {
        id
        title
      }
    }
  }
}
</page-query>

<script>
import Hero from "@/components/Hero"
import ProjectsGrid from "@/components/ProjectsGrid"
import LatestJournals from "@/components/LatestJournals"
export default {
  metaInfo () {
    return {
      title: 'Blog'
    }
  },
  components: {
    Hero,
    ProjectsGrid,
    LatestJournals
  }
}
</script>