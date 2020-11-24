<template>
  <article class="single">
      <div class="narrow">
          <h1 class="title" v-html="data.title.rendered"></h1>
        <div class="content" v-html="data.content.rendered"></div>
      </div>
    <div v-if="colorAccentStyles" v-html="colorAccentStyles"></div>
  </article>
</template>

<script>
import * as Vibrant from 'node-vibrant';

export default {
  props: {
    data: Object,
    type: String
  },

  head: {
    title: "Alexander Andreev"
  },

  mixins: {
    longTimestamp: Function
  },

  components: {
  },

  data() {
    return {
      expanded: false,
      colorAccentStyles: null
    };
  },

  methods: {
    initGallery() {
      let galleries = document.querySelectorAll('.content > .gallery');

      if (galleries.length) {
        if (process.browser) {
          require('lightgallery.js');
          require('lg-zoom.js');
          require('lg-thumbnail.js');
        }

        for (let i = 0; i < galleries.length; i++) {
          lightGallery(galleries[i], {
            download: false,
            selector: 'a'
          });
        }
      }
    },

    getColorAccentStyles(article) {
      return new Promise(function(resolve, reject) {
        const image =
          article._embedded['wp:featuredmedia'][0].media_details.sizes.thumbnail.source_url;

        Vibrant.from(image).getPalette((err, palette) => {
          if (!err && palette.DarkMuted) {
            const { r, g, b } = palette.DarkMuted;

            resolve(`
              <style>
                html,
                .featured-image .image-height {
                  background: rgb(${r},${g},${b}) !important
                }
                main a {
                  color: rgb(${r},${g},${b}) !important
                }
                main a:hover {
                  color: rgb(${r},${g},${b}) !important
                }
                main a::after {
                  background: rgb(${r},${g},${b}) !important
                }
              </style>
            `);
          } else {
            reject(err);
          }
        });
      });
    }
  },

  mounted() {
    this.initGallery();
  }
};
</script>

<style lang="scss" scoped>
@import '~/assets/css/vars.scss';

article {
  background-color: #fff;
  display: flex;
  flex-direction: column;
  position: relative;
  height: 100%;

  &.page-enter-active .narrow {
    transition: transform 1s cubic-bezier(0.11, 0.89, 0.31, 0.99), opacity 0.75s ease-out;
  }

  &.page-enter .narrow,
  .page-leave-to .narrow {
    transform: translateY(8px);
  }

  .narrow {
    margin: 0 auto;
    max-width: $containerWidth;
    min-height: calc(100vh - 60px - 60px);
    padding: 32px 64px 64px 64px;
    position: relative;
    transition: min-height 1s, transform 1s;
    transform: translateY(0);
    width: 100%;

    &.expanded {
      min-height: 0;
    }

    @media (max-width: 900px) {
      margin-top: 0 !important;
      max-width: 100%;
      min-height: initial;
    }

    @media (max-width: 700px) {
      max-width: none;
      padding: 0 16px 16px 16px;
    }

  }

  .title {
    font-size: 1.5rem;
    line-height: 1;
    margin-bottom: 16px;
    margin-top: 0;
    padding-top: 48px;
  }
}
</style>

<style lang="scss">
@import '../node_modules/lightgallery.js/dist/css/lightgallery.css';
@import '~/assets/css/vars.scss';

.lg-backdrop {
  background-color: #111;
}

#lg-counter {
  font-family: 'Roboto', sans-serif;
}

.lg-toolbar,
.lg-actions .lg-next,
.lg-actions .lg-prev,
.lg-outer .lg-thumb-outer,
.lg-outer .lg-toggle-thumb {
  background-color: #1a1a1a;
}

#lg-actual-size:after {
  content: '\E311';
}

#lg-zoom-in,
#lg-zoom-out {
  display: none;
}

.lg-outer .lg-thumb-item {
  border-radius: 2px;
}

.lg-outer .lg-thumb-item {
  border-color: #aaa;
}

.lg-outer .lg-thumb-item.active,
.lg-outer .lg-thumb-item:hover {
  border-color: #fff;
}
</style>

<style lang="scss">
@import '~/assets/css/vars.scss';

.single {
  .content {
    margin-top: 32px;

    p {
      max-width: 720px;
    }

    .wp-block-image {
      margin:0;
    }

    .alignnone,
    .size-full,
    .size-large,
    .wp-caption {
      background-color: #fff;
      padding: 4px 0px;
      display: block;
      margin-bottom: 32px;
      margin-inline-start: 0px;
      margin-inline-end: 0px;

      margin-block-start: 0em;
      margin-block-end: 0em;
      margin-inline-start: 0px;
      margin-inline-end: 0px;

      img {
        display: block;
        padding: 0;
        margin-bottom: 16px;
      }

      &.alignnone {
        max-width: 100%;
        float: left;
        
      }

      &.aligncenter {
        margin-left: auto;
        margin-right: auto;
      }

      &.alignleft {
        float: left;
        margin-right: 32px;
        width: initial;
      }

      &.alignright {
        float: right;
        margin-left: 32px;
        width: initial;
      }
    }

    .wp-caption {
      p {
        margin-bottom: 0;

        & + p {
          margin-top: 16px;
        }
      }
    }

    p {
      margin-bottom: 32px;
      margin-top: 0;
    }

    a {
      color: $primary;
      position: relative;

      &:hover {
        color: $accent;
      }

      &::after {
        background: rgba($accent, 0.5);
        content: '';
        height: 1px;
        left: 0;
        opacity: 0;
        position: absolute;
        top: 100%;
        transform: translateY(-4px);
        transition: height 0.1s, opacity 0.1s, transform 0.1s;
        width: 100%;
      }

      &:hover,
      &:focus {
        &::after {
          height: 4px;
          opacity: 1;
          transform: translateY(0px);
        }
      }
    }

    img {
      height: auto;
      max-width: 100%;
    }

    figure {
      margin-inline-start: 0px;
      margin-inline-end: 0px;
    }

    > *:first-child {
      margin-top: 0;
    }


  }
}
</style>
