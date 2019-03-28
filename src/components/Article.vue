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
    width: 80%;
    margin-bottom: 50px;
    margin-right: auto;
    margin-left: auto;
    box-shadow: 0px 0px 0px 0.1px black;
  }
  .article_title {
    font-size: 20px;
    font-weight: 600;
    margin-bottom: 10px;
    padding: 10px;
    box-shadow: 0px 0px 0px 0.1px black;
  }
  .article_subTitle {
    display: flex;
    justify-content: space-between;
    padding: 10px;
  }
  .article_contents {
    padding: 10px;
  }
  @media (max-width: 480px) {
    .article {
      width: 95%;
      margin-bottom: 20px;
    }
    .article_title {
      margin-bottom: 5px;
      padding: 5px;
    }
    .article_subTitle {
      padding: 5px;
    }
  }
</style>

