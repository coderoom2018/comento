<template>
  <div class="myPost_container">
    <h1>Post</h1>
    <div class="myPost" v-bind:article="article">
      title: {{ article.title }} <br>
      no: {{ article.no }} <br>
      email: {{ article.email }} <br>
      updated_at: {{ article.updated_at }} <br>
      contents: {{ article.contents }}

    </div>
    <div v-if="replies.length > 0">
      <div class="myReplies" v-for="reply in replies" v-bind:key="reply.index">
        reply: {{ reply }}
      </div>
    </div>
    <div v-else>
      남겨진 댓글이 없습니다.
    </div>
  </div>
</template>

<script>
export default {
  name: "Post",
  data: function() {
    return {
      article: this.article,
      replies: this.replies,
    };
  },
  props: ['article', 'replies'],
  mounted: function() {
    // this.getPost()
  },
  methods: {
    getPost: function() {
      const URI = "http://comento.cafe24.com/detail.php"
      const req_no = this.post_no

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
  },
};
</script>

<style scoped>
</style>

