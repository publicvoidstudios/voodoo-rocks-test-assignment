<template>
  <div
      class="container-fluid"
      v-if="users.length"
  >
    <div class="row">
      <post-item
          :post="post"
          :users="users"
          v-for="post in posts"
          :key="post.id"
          v-if="posts.length"
      ></post-item>
    </div>

    <div
        class="text-secondary text-center"
        v-if="!posts.length && searchInput.length"
    >
      No matches found for "{{searchInput}}"
    </div>

  </div>
</template>

<script>
import PostItem from "@/components/PostItem.vue";

export default {
  name: 'post-list',
  components: {PostItem},
  props: {
    posts: {
      type: Array,
      required: true
    },
    users: {
      type: Array,
      required: true
    },
    searchInput: {
      type: String,
      required: true,
      default: ''
    }
  }
}
</script>