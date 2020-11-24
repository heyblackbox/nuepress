<template>
  <header>
    <div>
      <span class="blog-title" to="/" @click.prevent="homeScrollTop">
        <h1 v-html="meta.name"></h1>
      </span>
      <nav>
        <nuxt-link to="/" exact>Works</nuxt-link>
        <nuxt-link to="/pages/info">Info</nuxt-link>
      </nav>
    </div>
  </header>
</template>

<script>

export default {
  components: {
  },

  computed: {
    meta() {
      return this.$store.state.meta;
    }
  },

  mixins: {
    homeScrollTop: Function
  }
};
</script>

<style lang="scss" scoped>
@import '~/assets/css/vars.scss';

header {
  font-family: 'Roboto', sans-serif;
  left: 0;
  position: fixed;
  top: 0;
  width: 100%;
  z-index: 10;

  > div {
    align-items: center;
    display: flex;
    justify-content: space-between;
    height: 60px;
    margin: 0 auto;
    // max-width: $containerWidth;
    padding: 0 24px;
    transition: padding-top 0.5s, padding-bottom 0.5s;

    @media (max-width: 700px) {
      height: 60px;
      padding: 16px;
    }
  }

  .blog-title {
    margin-right: 32px;

    @media (max-width: 700px) {
      margin-right: 16px;
    }

    h1 {
      color: $primary;
      cursor: pointer;
      font-size: 1rem;
      font-weight: 400;
    }
  }

  nav {
    flex-shrink: 0;
    @media (max-width: 500px) {

    }
  }

  a {
    color: $primary;
    border-color: lighten($primary, 40%);
    font-size: 1rem;
    font-weight: 400;
    text-decoration: none;
    transition: color 0.1s, border-color 0.1s;

    &:hover {

    }

    &.nuxt-link-active {
      padding-bottom: 3px;
      border-bottom: 1px solid;
    }

    & + a {
      margin-left: 24px;

      @media (max-width: 700px) {
        margin-left: 16px;
      }
    }
  }
}
</style>
