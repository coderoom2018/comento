<template>
  <div id="myMainContent">
    <Post 
      v-if="selectedPost_no.length > 0" 
      v-bind:article="article" 
      v-bind:replies="replies">
    </Post>

    <div v-else class="myPosts">
      <h1>Posts</h1>
      
      <div 
        class="myFilter" 
        v-for="category in categories" 
        v-bind:key="category.no"
        v-on:click="clickCategory(category.name)">
        <div>
          <button>{{ category.name }}</button>
        </div>
      </div>

      <div>
        <button v-on:click="clickAscendingBtn">오름차순</button>
        <button v-on:click="clickDescendingBtn">내림차순</button>
      </div>

      <div class="myPosts_container" v-for="post in posts" v-bind:key="post.no">
        <div class="myPost" v-on:click="clickPost(post.no)">
          <div class="myPost_mainHead">
            <div>{{ post.category_no }}</div>
            <div>no: {{ post.no }}</div>
          </div>
          <div class="myPost_subHead">
            {{ post.email }} |
            {{ post.updated_at }}
          </div>
          <div class="myPost_title">
            {{ post.title }}
          </div>
          <div class="myPost_contents">
            {{ post.contents }}
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
      memoriedPosts: this.memoriedPosts,
      classifiedPosts: [],
      categories: this.categories,
      selectedPost_no: "",
      article: {},
      replies: [],
      selectedCategory: "all",
      reverse: false,
      categoryClicked: false,
    }
  },
  components: {
    Post, Modal
  },
  props: [
    'posts', 'categories', 'memoriedPosts'
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
      console.log('memoriedPosts: ', this.memoriedPosts)
      this.posts = this.memoriedPosts.slice().filter(function(post) {
        return post.category_no === categoryName
      })
      console.log("newPosts: ", this.posts)
    },
    clickAscendingBtn: function() {
      if (!this.reverse) {
        this.posts = this.posts.slice().reverse()
        this.reverse = true;
      }
    },
    clickAscendingBtn: function() {
      if (!this.reverse) {
        this.posts = this.posts.slice().reverse()
        this.reverse = true;
      }
    },
    clickDescendingBtn: function() {
      if (this.reverse) {
        this.posts = this.posts.slice().reverse()
        this.reverse = false;
      }
    }
  },
  computed: {
  }
}
</script>

<style scoped>
  .myFilter {
    display: flex;
    flex-direction: row;
  }
  .myPosts_container {
    padding-bottom: 100px;
  }
  .myPost {
    display: flex;
    flex-direction: column;
  }
  .myPost_mainHead {
    display: flex;
    justify-content: space-between;
    font-weight: 500;
  }
  .myPost_subHead {
    display: flex;
    font-weight: 500;
  }
  .myPost_title {
    font-size: 25px;
    font-weight: 600;
  }
  .myPost_contents {
  }

</style>

