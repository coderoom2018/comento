<template>
  <div class="myReplies_container">
    <h2>Replies</h2>
    <div v-if="replies.length > 0">
      <div class="myReplies" v-for="reply in replies" v-bind:key="reply.index">
        <div class="myReplies_head">
          <div>{{ reply.email }}</div>
          <div>{{ reply.updated_at }}</div>
        </div>
        <div class="myReplies_contents">
          {{ reply.contents }}
        </div>
      </div>
    </div>

    <div v-else class="noneReplies">남겨진 댓글이 없습니다.</div>
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
  .myReplies {
    display: flex;
    flex-direction: column;
    margin-bottom: 30px;
    width: 80%;
    margin-right: auto;
    margin-left: auto;
    box-shadow: 0px 0px 0px 0.1px black;
  }
  .myReplies_head {
    display: flex;
    justify-content: space-between;
    font-weight: 600;
    box-shadow: 0px 0px 0px 0.1px black;
    padding: 10px;
  }
  .myReplies_contents {
    padding: 10px;
  }
  .noneReplies {
    height: 200px;
  }
  @media (max-width: 480px) {
    .myReplies {
      width: 95%;
      margin-bottom: 20px;
    }
    .myReplies_head {
      padding: 5px;
    }
    .myReplies_contents {
      padding: 5px;
    }
  }
</style>

