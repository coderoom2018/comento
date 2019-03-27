<template>
  <div id="myMainContent">
    <Post 
      v-if="selectedPost_no.length > 0" 
      v-bind:article="article" 
      v-bind:replies="replies">
    </Post>

    <div v-else class="myPosts" display="none">
      <h1>Posts</h1>
      
      <div 
        class="myFilter" 
        v-for="category in categories" 
        v-bind:key="category.no"
        v-on:click="clickCategory(category.name)">
        <button>{{ category.name }}</button>
      </div>

      <div class="myPosts_container" v-for="post in posts" v-bind:key="post.no">
        <div 
          class="myPost" 
          v-on:click="clickPost(post.no)"
          v-if="!hidden">
          <div class="myPost_mainHead">
            <div>category_no: {{ post.category_no }}</div>
            <div>no: {{ post.no }}</div>
          </div>
          <div class="myPost_subHead">
            email: {{ post.email }} |
            updated_at: {{ post.updated_at }}
          </div>
          <div class="myPost_title">
            <h3>title: {{ post.title }}</h3>
          </div>
          <div class="myPost_contents">
            content: {{ post.contents }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Post from './Post'
import Modal from './Post'

export default {
  name: 'Posts',
  data: function() {
    return {
      posts: this.posts,
      categories: this.categories,
      selectedPost_no: "",
      article: {},
      replies: [],
      selectedCategory: "all",
    }
  },
  components: {
    Post, Modal
  },
  props: [
    'posts', 'categories'
  ],
  methods: {
    clickPost: function(selectedPost_no) {
      this.selectedPost_no = selectedPost_no

      const URI = "http://comento.cafe24.com/detail.php"
      const req_no = selectedPost_no

      this.$http.get(`${URI}?req_no=${req_no}`)
      .then(result => {
        console.log("selectedPost: ", result.data.detail)
        this.article = result.data.detail.article
        this.replies = result.data.detail.replies
      })
      .catch(error => {
        console.log(error);
      });
    },
    clickCategory: function(categoryName) {
      console.log(categoryName)
      // let posts = this.posts
      
      // posts.filter(post => {
      //   post.category_no === categoryName
      // }) 

      // this.posts = posts

      // console.log(this.posts)
    },
  },
  computed: {
  }
}
</script>

<style scoped>

</style>

