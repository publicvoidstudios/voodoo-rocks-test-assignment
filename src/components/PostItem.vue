<template>
  <div class="container m-0 py-2 col-xs-12 col-sm-6 col-md-4 col-lg-3 col-xxl-2">
    <div class="m-0 bg-white p-3 shadow-sm">
      <h2 class="fw-bold h5 text-primary capitalize-first-letter">
        {{ post.title }}
      </h2>
      <p class="text-secondary-emphasis capitalize-first-letter mb-3">
        {{ post.body }}
      </p>
      <p class="text-secondary m-0">
        {{ getUserName(post.userId) }}
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'post-item',
  props: {
    post: {
      required: true,
      default: {
        id: 0,
        userId: 0,
        title: '',
        body: ''
      }
    },
    users: {
      type: Array,
      required: true
    }
  },
  methods: {
    //Gets user's name by user's id
    getUserName(userId) {
      const user = this.users.find(user => user.id === userId);
      return user.name;
    }
  }
}
</script>