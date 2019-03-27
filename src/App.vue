<template>
  <div id="app">
    <Posts />
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
      categories: [],
      posts: [],
    }
  },
  mounted: function() {
    this.getCategories()
    this.getPosts()
  },

  methods: {
    getCategories: function() {
      const URI = 'http://comento.cafe24.com/category.php'

      this.$http.get(URI)
      // this.$http.get('http://comento.cafe24.com/category.php')
      .then(result => {
        console.log('category list: ', result.data)
        const data = result.data
        this.categories = data.list
      })
      .catch(error => {
        console.log(error)
      })
    },

    getPosts: function() {
      const URI = 'http://comento.cafe24.com/request.php'
      const page = '1'

      this.$http.get(`${URI}?page=${page}`)
      .then(result => {
        console.log(result.data)
        this.posts = result.data
      })
    }
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
