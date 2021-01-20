<template>
  <div class="article-meta">
    <RwvArticleInfo :article="article" />
    <RwvArticleButtons :article="article" />
    <router-link :to="articleLink(article)" class="preview-link">
      <h1 v-text="article.title" />
      <p v-text="article.description" />
      <span>Read more...</span>
      <TagList :tags="article.tagList" />
    </router-link>
  </div>
</template>

<script>
import TagList from "./TagList";
import RwvArticleInfo from "./ArticleInfo";
import RwvArticleButtons from "./ArticleButtons";

export default {
  name: "RwvArticleCard",
  components: {
    TagList,
    RwvArticleInfo,
    RwvArticleButtons
  },
  props: {
    article: {
      type: Object,
      required: true
    }
  },
  data() {
    return {};
  },
  methods: {
    articleLink(article) {
      return {
        name: "article",
        params: {
          slug: article.slug
        }
      };
    }
  }
};
</script>

<style lang="css" scoped></style>
