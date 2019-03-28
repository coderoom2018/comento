<template>
  <div id="myMainContent">
    <Post 
      v-if="selectedPost_no.length > 0" 
      v-bind:article="article" 
      v-bind:replies="replies">
    </Post>

    <div v-else class="myPosts">
      <h1>Posts</h1>
      <div class="myFilter_container">
        <div 
          class="myFilter" 
          v-for="category in categories" 
          v-bind:key="category.no"
          v-on:click="clickCategory(category.name)">
            <div class="categoryBtn">{{ category.name }}</div>
        </div>
      </div>

      <div class="orderingBtn_container">
        <div class="orderingBtn" v-on:click="clickAscendingBtn">오름차순</div>
        <div class="orderingBtn" v-on:click="clickDescendingBtn">내림차순</div>
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
          <div class="myPost_content">
            <div class="myPost_title">
              {{ post.title }}
            </div>
            <div class="myPost_contents">
              {{ post.shortenContents }} ...
            </div>
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
        this.article = result.data.detail.article
        this.replies = result.data.detail.replies
      })
      .catch(error => {
        console.log(error);
      });
    },
    clickCategory: function(categoryName) {
      this.posts = this.memoriedPosts.slice().filter(function(post) {
        return post.category_no === categoryName
      })
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
}
</script>

<style scoped>
  .myFilter_container {
    display: flex;
  }
  .categoryBtn {
    font-size: 15px;
    background-color: white;
    box-shadow: 0px 0px 0px 0.1px black;
    height: 25px;
    width: 100px;
    margin-left: 15px;
  }
  .categoryBtn:hover {
    cursor: pointer;
  }
  .myPosts_container {
    padding-bottom: 50px;
  }
  .myPost {
    display: flex;
    flex-direction: column;
    width: 80%;
    margin-right: auto;
    margin-left: auto;
    box-shadow: 0px 0px 0px 0.1px black;
  }
  .myPost_mainHead {
    display: flex;
    justify-content: space-between;
    font-weight: 500;
    box-shadow: 0px 0px 0px 0.1px black;
    padding: 10px;
  }
  .myPost_subHead {
    display: flex;
    font-weight: 500;
    padding: 10px 0 0 10px;
  }
  .myPost_content {
    padding: 10px;
  }
  .myPost_content:hover {
    cursor: pointer;
  }
  .myPost_title {
    font-size: 25px;
    font-weight: 600;
  }
  .orderingBtn_container {
    display: flex;
    justify-content: flex-end;
    padding: 20px;
  }
  .orderingBtn {
    font-size: 15px;
    background-color: white;
    box-shadow: 0px 0px 0px 0.1px black;
    height: 25px;
    width: 100px;
    margin-left: 15px;
  }
  .orderingBtn:hover {
    cursor: pointer;
  }
  @media (max-width: 480px) {
    .myPost {
      width: 95%;
    }
  }

</style>

