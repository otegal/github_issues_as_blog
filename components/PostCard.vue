<template>
  <div>
    <v-card
      class="mx-auto"
      outlined
      v-for="post in posts"
      :key="post.id"
    >
      <v-list-item three-line>
        <v-list-item-title class="headline mb-1">{{ post.title }}</v-list-item-title>
        <v-list-item-subtitle>created_at:{{ post.createdAt }}</v-list-item-subtitle>
      </v-list-item>
    </v-card>
  </div>
</template>

<script>
export default {
  props: ['posts']
}
</script>