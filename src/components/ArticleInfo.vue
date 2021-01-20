<template>
  <span>
    <router-link
      :to="{ name: 'profile', params: { username: article.author.username } }"
    >
      <img :src="article.author.image" />
    </router-link>
    <div class="info">
      <router-link
        :to="{ name: 'profile', params: { username: article.author.username } }"
        class="author"
      >
        {{ article.author.username }}
      </router-link>
      <span class="date">{{ article.createdAt | date }}</span>
    </div>
  </span>
</template>

<script>
export default {
  name: "RwvArticleInfo",

  props: {
    article: {
      type: Object,
      required: true
    }
  }
};
</script>

<style lang="css" scoped></style>
