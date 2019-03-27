<template>
  <div class="myReplies_container">
    <div v-if="replies.length > 0">
      <div class="myReplies" v-for="reply in replies" v-bind:key="reply.index">
        <div class="myHead_contents">
          email: {{ reply.email }}<br>
          updated_at: {{ reply.updated_at }} <br>
        </div>
        <div class="myMain_contents">
          contents: {{ reply.contents }}<br>
        </div>
      </div>
    </div>
    
    <div v-else>남겨진 댓글이 없습니다.</div>
  </div>
</template>

<script>
export default {
  name: "Replies",
  data: function() {
    return {
      replies: this.replies
    };
  },
  props: [
    'replies'
  ]
};
</script>

<style scoped>
</style>

