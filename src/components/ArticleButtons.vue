<template>
  <span>
    <span v-if="isCurrentUser(article)">
      <router-link
        class="btn btn-sm btn-outline-secondary"
        :to="editArticleLink"
      >
        <i class="ion-edit"></i> <span>&nbsp;Edit Article</span>
      </router-link>
      <span>&nbsp;&nbsp;</span>
      <button class="btn btn-outline-danger btn-sm" @click="deleteArticle">
        <i class="ion-trash-a"></i> <span>&nbsp;Delete Article</span>
      </button>
    </span>
    <!-- Used in ArticleView when not author -->
    <span v-else>
      <button class="btn btn-sm btn-outline-secondary" @click="toggleFollow">
        <i class="ion-plus-round"></i> <span>&nbsp;</span>
        <span v-text="followUserLabel(article)" />
      </button>
      <span>&nbsp;&nbsp;</span>
      <button
        class="btn btn-sm"
        @click="toggleFavorite"
        :class="toggleFavoriteButtonClasses(article)"
      >
        <i class="ion-heart"></i> <span>&nbsp;</span>
        <span v-text="favoriteArticleLabel(article)" /> <span>&nbsp;</span>
        <span class="counter" v-text="favoriteCounter(article)" />
      </button>
    </span>
  </span>
</template>

<script>
import { mapGetters } from "vuex";

import {
  FAVORITE_ADD,
  FAVORITE_REMOVE,
  ARTICLE_DELETE,
  FETCH_PROFILE_FOLLOW,
  FETCH_PROFILE_UNFOLLOW
} from "../store/actions.type";

export default {
  name: "RwvArticleButtons",

  props: {
    article: {
      type: Object,
      required: true
    }
  },
  computed: {
    editArticleLink() {
      return { name: "article-edit", params: { slug: this.article.slug } };
    },
    ...mapGetters(["currentUser", "profile", "isAuthenticated"])
  },
  methods: {
    isCurrentUser(article) {
      if (this.currentUser.username && article.author.username) {
        return this.currentUser.username === article.author.username;
      }
      return false;
    },
    async deleteArticle() {
      try {
        await this.$store.dispatch(ARTICLE_DELETE, this.article.slug);
        this.$router.push("/");
      } catch (err) {
        console.error(err);
      }
    },
    toggleFavorite() {
      if (!this.isAuthenticated) {
        this.$router.push({ name: "login" });
        return;
      }
      const action = this.article.favorited ? FAVORITE_REMOVE : FAVORITE_ADD;
      this.$store.dispatch(action, this.article.slug);
    },
    toggleFollow() {
      if (!this.isAuthenticated) {
        this.$router.push({ name: "login" });
        return;
      }
      const action = this.article.following
        ? FETCH_PROFILE_UNFOLLOW
        : FETCH_PROFILE_FOLLOW;
      this.$store.dispatch(action, {
        username: this.profile.username
      });
    },
    toggleFavoriteButtonClasses(article) {
      return {
        "btn-primary": article.favorited,
        "btn-outline-primary": !article.favorited
      };
    },
    followUserLabel(article) {
      return `${this.profile.following ? "Unfollow" : "Follow"} ${
        article.author.username
      }`;
    },
    favoriteArticleLabel(article) {
      return article.favorited ? "Unfavorite Article" : "Favorite Article";
    },
    favoriteCounter(article) {
      return `(${article.favoritesCount})`;
    }
  }
};
</script>

<style lang="css" scoped></style>
