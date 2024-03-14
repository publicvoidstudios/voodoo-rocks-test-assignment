<template>
  <div class="container-fluid d-flex flex-column align-items-center">
    <search-bar @searchInput="handleSearch"></search-bar>
    <loading-spinner v-if="!shownPosts.length && !searchInput.length"></loading-spinner>
    <post-list :posts="shownPosts" :users="users" :searchInput="searchInput"></post-list>
  </div>
</template>

<script>

import PostList from "@/components/PostList.vue";
import SearchBar from "@/components/SearchBar.vue";
import LoadingSpinner from "@/components/LoadingSpinner.vue";

export default {
  components: {LoadingSpinner, SearchBar, PostList},
  data () {
    return {
      posts: [],
      users: [],
      shownPosts: [],
      searchInput: ''
    }
  },
  methods: {
    //Get posts from json-placeholder and store them in posts array
    async fetchPosts() {
      await fetch('https://jsonplaceholder.typicode.com/posts')
          .then(response => response.json())
          .then(json => this.posts = json);
    },
    //Get users from json-placeholder and store them in users array
    async fetchUsers() {
      await fetch('https://jsonplaceholder.typicode.com/users')
          .then(response => response.json())
          .then(json => this.users = json);
    },
    //Handle search input
    handleSearch(input) {
      //Assign search input string to searchInput to be able to use it further
      this.searchInput = input;
      //If search input is empty -> use all posts
      if(input.trim() === ''){
        this.useAllPosts();
      } else {
        //Filter posts using search input and store filtered posts, that are going to be shown, in shownPosts array
        this.shownPosts = this.posts.filter(post => this.users.filter(user => user.name.toLowerCase().includes(input.toLowerCase())).some(user => user.id === post.userId));
      }
    },
    //Makes all posts visible by using all posts in shownPosts
    useAllPosts() {
      this.shownPosts = this.posts;
    },
    //Wait for all data to be received and then use it in app
    async readyData() {
      try {
        await this.fetchUsers();
        await this.fetchPosts();
        this.waitForPosts();
      } catch (e) {
        console.log(`Failed to retrieve data from server with error: ${e}`);
      }
    },
    //Wait for posts to fill with data and use posts when ready
    waitForPosts() {
      //If posts are not empty
      if (this.posts.length) {
        //Use posts in app
        this.useAllPosts();
      } else {
        //Reset function with small timeout
        setTimeout(this.waitForPosts, 100);
      }
    }
  },
  mounted() {
    //Get and use data on app start
    this.readyData();
  }
}
</script>

<style>
/* Reset */
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

/*region Custom classes */

/* First letter capitalization */
.capitalize-first-letter:first-letter {
  text-transform: capitalize;
}

/*endregion*/
</style>
