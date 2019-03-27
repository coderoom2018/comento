<template>
  <div id="app">
    <Posts v-bind:posts='posts' v-bind:categories='categories' />
    <Ads />
  </div>
</template>

<script>
import Posts from './components/Posts';
import Post from './components/Post';
import Modal from './components/Modal';
import Ads from './components/Ads';

export default {
  name: 'App',
  components: {
    Ads, Modal, Post, Posts
  },
  data: function() {
    return {
      posts: [],
      categories: [],
    }
  },
  mounted: function() {
    this.getPosts()
  },
  methods: {
    getPosts: function() {
      const categoriesURI = 'http://comento.cafe24.com/category.php'

      this.$http.get(categoriesURI)
      .then(result => {
        console.log('category list: ', result.data)
        this.categories = result.data.list
      })
      .then(() => {
        const postsURI = 'http://comento.cafe24.com/request.php'
        const postsPage = '1'
  
        this.$http.get(`${postsURI}?page=${postsPage}`)
        .then(result => {
          console.log("Posts: ", result.data.list)

          let list = result.data.list
  
          list.map(post => {
            for (var i = 0; i < this.$data.categories.length; i++) {
              if (post.category_no === this.$data.categories[i].no) {
                post.category_no = this.$data.categories[i].name
              }
            }  
          })
          this.posts = list
        })
      })
      .catch(error => {
        console.log(error)
      })
    },
  },
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
* {
  box-sizing: border-box;
  box-shadow: 0px 0px 0px 0.1px black;
}
</style>
