<template>
  <div id="app">
    <Posts 
      v-bind:posts='posts' 
      v-bind:memoriedPosts='posts' 
      v-bind:categories='categories' />
    <div class="addMoreDataBtn_container">
      <div class="addMoreDataBtn" v-on:click="getPosts">더 불러오기</div>
    </div>
    <Ads v-bind:ads="ads"/>
  </div>
</template>

<script>
import Posts from './components/Posts';
import Post from './components/Post';
import Ads from './components/Ads';

export default {
  name: 'App',
  components: {
    Ads, Post, Posts
  },
  data: function() {
    return {
      postsLoadingCount: 1,
      posts: [],
      ads: [],
      categories: [],
    }
  },
  mounted: function() {
    this.getPosts()
  },
  methods: {
    getPosts: function() {
      this.getAds()

      const categoriesURI = 'http://comento.cafe24.com/category.php'

      this.$http.get(categoriesURI)
      .then(result => {
        this.categories = result.data.list
      })
      .then(() => {
        const postsURI = 'http://comento.cafe24.com/request.php'
        const postsPage = this.postsLoadingCount + ''
  
        this.$http.get(`${postsURI}?page=${postsPage}`)
        .then(result => {
          let list = result.data.list
  
          list.map(post => {
            for (var i = 0; i < this.$data.categories.length; i++) {
              if (post.category_no === this.$data.categories[i].no) {
                post.category_no = this.$data.categories[i].name
                post.shortenContents = post.contents.slice(0, 400)
              }
            }  
          })
          this.posts = this.posts.concat(list)
          this.postsLoadingCount = this.postsLoadingCount + 1 + ''
        })
      })
      .catch(error => {
        console.log(error)
      })
    },
    
    getAds: function() {
      const URI = "http://comento.cafe24.com/ads.php";
      const page = this.postsLoadingCount;
      const limit = 2;

      this.$http
        .get(`${URI}?page=${page}&limit=${limit}`)
        .then(result => {
          let list = result.data.list

          list.map(ad => {
            ad.img = `http://comento.cafe24.com/public/images/${ad.img}`
          })
          this.ads = this.ads.concat(list)
        })
        .catch(error => {
          console.log(error);
        });
    },
  }
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
  }
  .addMoreDataBtn_container {
    display: flex;
    justify-content: center;
  }
  .addMoreDataBtn {
    font-size: 20px;
    background-color: white;
    box-shadow: 0px 0px 0px 0.1px black;
    height: 30px;
    width: 60%;
  }
  .addMoreDataBtn:hover {
    cursor: pointer;
  }
</style>
