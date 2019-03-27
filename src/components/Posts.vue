<template>
  <div id="myMainContent">
    <div>
      <button>필터</button>
    </div>
    <Post 
      v-if="selectedPost_no.length > 0" 
      v-bind:article="article" 
      v-bind:replies="replies">
    </Post>

    <div v-else class="myPosts">
      <h1>Posts</h1>
      <div class="myPosts_container" v-for="post in posts" v-bind:key="post.no">
        <div class="myPost" v-on:click="clickPost(post.no)">
          <div class="myPost_mainHead">
            category_no: {{ post.category_no }} 
            no: {{ post.no }}
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

export default {
  name: 'Posts',
  data: function() {
    return {
      posts: [],
      categories: [],
      ads: [],
      selectedPost_no: "",
      article: {},
      replies: [],
    }
  },
  components: {
    Post,
  },
  mounted: function() {
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
      const postsURI = 'http://comento.cafe24.com/request.php'
      const postsPage = '1'

      this.$http.get(`${postsURI}?page=${postsPage}`)
      .then(result => {
        console.log("Posts: ", result.data.list)
        this.posts = result.data.list
      })
      .catch(error => {
        console.log(error)
      })
    },

    clickPost: function(selectedPost_no) {
      console.log("selectedPost_no: ", typeof this.$data.selectedPost_no)
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
    }
  },
  created: function() {
    // let firstAd = document.getElementsByClassName("myAds_container")[0]
    // let secondAd = document.getElementsByClassName("myAds_container")[1]

    // let firstTarget = document.getElementsByClassName("myPosts_container")[3]
    // // document.getElementsByClassName("myPosts_container")[3].appendChild(firstAd)

    // let secondTarget = document.getElementsByClassName("myPosts_container")[7]

    // console.log("firstAd: ", firstAd)
    // console.log("secondAd: ", secondAd)
    // console.log("firstTarget: ", firstTarget)
    // console.log("secondTarget: ", secondTarget)

    // document.getElementsByClassName("myPosts_container")[3].appendChild(document.getElementsByClassName("myAds_container")[0])
    
    // this.$nextTick(function() {

    // })
  },
}

</script>

<style scoped>

</style>

