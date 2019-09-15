<template>
  <v-content>
    <PostCard :posts="posts" />
  </v-content>
</template>

<script>
import PostCard from '~/components/PostCard.vue'
import axios from 'axios'

export default {
  data () {
    return {
      posts: []
    }
  },
  components: {
    PostCard
  },
  mounted () {
    console.log("ss")
    const baseUrl = 'https://api.github.com/graphql'
    const token = process.env.GITHUB_PERSONAL_ACCESS_TOKEN
    const owner = process.env.GITHUB_OWNER
    const repositoryName = process.env.GITHUB_REPOSITORY_NAME
    // TODO queryビルディング方法を変えたい
    const postQuery = `query {
      repository(owner:"${owner}", name:"${repositoryName}") {
        name,
        issues(last:3, states:OPEN) {
          totalCount,
          pageInfo {
            endCursor
            startCursor
          }
          nodes {
            id,
            title,
            createdAt,
            updatedAt,
            author {
              login
            }
          }
        }
      }
    }`
    axios.post(baseUrl, {
      query: postQuery
    }, {
      headers: {
        Authorization: `Bearer ${token}`
      }
    })
      .then((response) => {
        console.log(response)
        console.log(response.data.data.repository.issues.nodes)
        this.posts = response.data.data.repository.issues.nodes
      })
      .catch(() => {
        console.log('Github APIのエラー')
      })
  }
}
</script>
