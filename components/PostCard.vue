<template>
  <div>
    <v-flex xs12>
      <v-card
        class="mx-auto"
        outlined
        v-for="post in posts"
        :key="post.munber"
        @click="moveToDetail(post.number)"
      >
        <v-list-item three-line>
          <v-list-item-title class="headline mb-1">{{ post.title }}</v-list-item-title>
          <v-list-item-subtitle>created_at:{{ post.createdAt }}</v-list-item-subtitle>
        </v-list-item>
      </v-card>
    </v-flex>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  props: ['posts'],
  methods: {
    moveToDetail (issueNumber) {
      this.$router.push('../detail/?issueNumber='+issueNumber)
    }
  }
}
</script>