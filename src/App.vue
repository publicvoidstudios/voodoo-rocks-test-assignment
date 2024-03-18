<template>
  <div class="container-fluid d-flex flex-column align-items-center">
    <search-bar @searchInput="handleInput" v-if="!errors.length"></search-bar>
    <loading-spinner v-if="!displayedPosts.length && !searchInput.length && !errors.length"></loading-spinner>
    <post-list :posts="displayedPosts" :users="users" :searchInput="searchInput" :errors="errors"></post-list>
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
      searchInput: '',
      errors: []
    }
  },
  methods: {
    // Method to get data from API URL and store it in a variable
    async fetchData(url) {
      try {
        const response = await fetch(url);
        if (!response.ok) {
          throw new Error(`Failed to fetch data from ${url}`);
        }
        return await response.json();
      } catch (error) {
        console.error(`Error fetching data from ${url}. Error: ${error.message}`);
        this.errors.push({
          message: `Error fetching data from ${url}. Error: ${error.message}`
        });
        return error;
      }
    },

    //Method to store input data received from event
    handleInput(input) {
      this.searchInput = input;
    },

    //Wait for all data to be received and then use it in app
    async readyData() {
        this.posts = await this.fetchData('https://jsonplaceholder.typicode.com/posts');
        this.users = await this.fetchData('https://jsonplaceholder.typicode.com/users');
    }
  },
  computed: {
    displayedPosts() {
      if(this.searchInput.trim() === ''){
        return this.posts;
      } else {
        //Filter posts using search input and store filtered posts, that are going to be shown, in shownPosts array
        return this.posts.filter(post => this.users.filter(user => user.name.toLowerCase().includes(this.searchInput.toLowerCase())).some(user => user.id === post.userId));
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
