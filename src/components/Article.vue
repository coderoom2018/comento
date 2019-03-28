<template>
  <div class="article" v-bind:article="article">
    <div class="article_title">
      {{ article.title }} <br>
    </div>
    <div class="article_subTitle">
      <div>{{ article.no }}</div>
      <div>{{ article.email }}</div>
      <div>{{ article.updated_at }}</div>
    </div>
    <div class="article_contents">
      {{ article.contents }}
    </div>
  </div>
</template>

<script>
export default {
  name: "Article",
  data: function() {
    return {
      article: this.article,
    };
  },
  props: [
    'article'
  ],
};
</script>

<style scoped>
  .article {
    display: flex;
    flex-direction: column;
    padding-bottom: 50px;
    width: 80%;
    margin-right: auto;
    margin-left: auto;
  }
  .article_title {
    font-size: 20px;
    font-weight: 600;
    padding-bottom: 10px;
  }
  .article_subTitle {
    display: flex;
    justify-content: space-between;
    padding-bottom: 10px;
  }
</style>

