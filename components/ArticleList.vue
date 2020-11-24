<template>
  <div class="article-list">
    <article v-for="article in articles" :key="article.id">
      <nuxt-link :to="`/${article.slug}`" class="row">
          <div class="lazy medium" v-if="article._embedded['wp:featuredmedia']">
            <img
              :alt="article._embedded['wp:featuredmedia'][0].alt_text"
              v-lazy="
                article._embedded['wp:featuredmedia'][0].media_details.sizes.large.source_url
              "
            />

          </div>
        <div class="caption">
          <p v-html="article.title.rendered"></p>
        </div>
      </nuxt-link>
    </article>
  </div>
</template>

<script>
import Spinner1 from '~/components/Spinner1';

export default {
  components: {
    Spinner1
  },
  props: {
    articles: Array
  },
  mixins: {
    shortTimestamp: Function
  }
};
</script>

<style lang="scss" scoped>
@import '~/assets/css/vars.scss';

.article-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: stretch;

  article {
    width: 50%;
    max-height: 60%;
    padding: 2% 5%;
    text-align: center;

    &:hover {

    }

    @media (max-width: 900px) {
      width: 100%;
      padding: 10%;
    }
  }

  .row {
    display: flex;
    height: 100%;
    flex-direction:column;
    justify-content:center;
    text-align:center;
    margin: auto;

    color: #606060;

    .col {
      @media (max-width: 700px) {
        & + .col {
          margin-top: 16px;
        }
      }
    }
  }

  .caption {
    text-align: center;
    font-size: 1rem;
    margin-top: 0.5rem;
  }


  .lazy {
      margin: 0 0 0 0;
      align-self: center;
      background: none;
      min-height: 200px;

    &.medium {
      display: block;
    }

    @media (max-width: 700px) {
      margin: 0;
      min-height: 0;

    }

    img {
      object-fit: contain;
      max-height: 600px;
      background-color: none;

      &:hover {
        transition: all 1s cubic-bezier(0.11, 0.89, 0.31, 0.99);
        opacity: 0.92;
      }

      @media (max-width: 700px) {
        object-fit: cover;
        width: 100%;
      }
    }

    .spinner {
      @media (max-width: 500px) {
        transform: scale(0.4);
      }
    }
  }

  a {
    color: $primary;
    transition: 0.2s;
    text-decoration: none;

    &:hover {

    }
  }

  p {
    margin: 0;
  }
}
</style>
