<template>
  <v-flex xs12>
    <h2>{{ issue.title }}</h2>
    <br>
    <div v-html="$md.render(issueBody)"></div>
  </v-flex>
</template>

<script>
import axios from 'axios'

export default {
  data () {
    return {
      issueNumber: 0,
      issue: {},
      issueBody: ''
    }
  },
  asyncData (context) {
    return {
      issueNumber: context.query.issueNumber
    }
  },
  mounted () {
    const baseUrl = 'https://api.github.com/graphql'
    const token = process.env.GITHUB_PERSONAL_ACCESS_TOKEN
    const owner = process.env.GITHUB_OWNER
    const repositoryName = process.env.GITHUB_REPOSITORY_NAME
    // TODO queryビルディング方法を変えたい
    const postQuery = `query {
      repository(owner:"${owner}", name:"${repositoryName}") {
        issue(number:${this.issueNumber}) {
          title,
          body
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
        console.log(response.data)
        this.issue = response.data.data.repository.issue
        // bodyはこれで登録しないとString型を$md.renderに渡せていないよエラーが出るための暫定対応
        this.issueBody = response.data.data.repository.issue.body
        console.log(this.issue)
      })
      .catch(() => {
        console.log('Github APIのエラー')
      })
  }
}
</script>