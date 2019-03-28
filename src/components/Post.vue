<template>
  <div class="myPost_container">
    <h1>Post</h1>
    <Article v-bind:article="article" />
    <Replies v-bind:replies="replies" />
  </div>
</template>

<script>
import Article from './Article'
import Replies from './Replies'

export default {
  name: "Post",
  data: function() {
    return {
      article: this.article,
      replies: this.replies,
    };
  },
  components: {
    Article, Replies
  },
  props: [
    'article', 'replies'
  ],
};
</script>

<style scoped>
</style>

