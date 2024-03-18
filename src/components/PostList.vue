<template>
  <div
      class="container-fluid"
      v-if="!errors.length && users.length && posts.length"
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

  <div
    v-if="errors.length"
    v-for="error in errors"
    class="text-danger border border-2 border-danger rounded bg-dark text-start m-2 p-2"
  >
    <p>An error occurred when receiving data from server.</p>
    <p class="text-light">{{error.message}}</p>
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
    },
    errors: {
      type: Array,
      default: [],
      required: true
    }
  }
}
</script>