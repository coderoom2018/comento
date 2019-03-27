<template>
  <div class="article" v-bind:article="article">
    title: {{ article.title }} <br>
    no: {{ article.no }} <br>
    email: {{ article.email }} <br>
    updated_at: {{ article.updated_at }} <br>
    contents: {{ article.contents }}
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
</style>

