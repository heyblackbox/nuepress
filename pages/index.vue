<template>
  <div class="home">
    <div class="articles">
      <ArticleList :articles="[...articlesList].slice(0)" />
      <client-only>
        <InfiniteLoading ref="infiniteLoading" @infinite="moreArticles">
          <span slot="no-results">
            <div>&nbsp;</div>
          </span>
        </InfiniteLoading>
      </client-only>
    </div>
  </div>
</template>

<script>
import ArticleList from '~/components/ArticleList';
import TheHero from '~/components/TheHero';
import InfiniteLoading from 'vue-infinite-loading';
import Smile from '~/assets/svg/Smile.vue';
import Spinner1 from '~/components/Spinner1.vue';

export default {
  async asyncData({ app, store, params }) {
    const { data } = await app.$axios.get(
      `${process.env.WORDPRESS_API_URL}/wp/v2/posts?orderby=date&per_page=10&_embed`
    );
    return { articles: data };
  },

  components: {
    ArticleList,
    TheHero,
    InfiniteLoading,
    Smile,
    Spinner1
  },

  computed: {
    articlesList() {
      return [...this.articles].filter(
        article => !article.categories.includes(parseInt(process.env.FEATURED_CATEGORY_ID))
      );
    }
  },

  data() {
    return {
      infiniteLoadingPage: 1
    };
  },

  head() {
    return {
      title: `${this.$store.state.meta.name} | Photography`,
      meta: [{ description: this.$store.state.meta.description }]
    };
  },

  methods: {
    moreArticles($state) {
      this.$axios
        .get(`${process.env.WORDPRESS_API_URL}/wp/v2/posts`, {
          params: {
            orderby: 'date',
            per_page: 10,
            categories_exclude: process.env.FEATURED_CATEGORY_ID,
            page: this.infiniteLoadingPage+1,
            _embed: true
          }
        })
        .then(response => {
          this.articles = [...this.articles, ...response.data];
          this.infiniteLoadingPage++;
          $state.loaded();
        })
        .catch(() => $state.complete());
    }
  }
};
</script>

<style lang="scss" scoped>
@import '~/assets/css/vars.scss';

.home {
  display: flex;
  background: #ffffff;

  .articles {
    padding: 0 auto;
    margin: 0 auto;
//    max-width: $containerWidth;
    width: 100%;

    @media (max-width: 1000px) {
      max-width: none;
    }

    @media (max-width: 700px) {
      padding: 0 16px;
    }

    .article-list {
      margin: 32px 0;

      @media (max-width: 700px) {
        margin: 16px 0;
      }
    }
  }
}
</style>
