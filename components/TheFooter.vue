<template>
  <footer>
    <div><a href="http://instagram.com/heyblackbox" target="_blank">Instagram</a></div>
    <div>© {{ year }}</div>
  </footer>
</template>

<script>
export default {
  computed: {
    meta() {
      return this.$store.state.meta;
    },
    year() {
      return new Date().getFullYear();
    }
  },

  mixins: {
    homeScrollTop: Function
  }
};
</script>

<style lang="scss" scoped>
@import '~/assets/css/vars.scss';

footer {
  display: flex;
  justify-content: space-between;
  > div {
    align-items: center;
    display: flex;
    height: 60px;
    margin: 0;
    padding: 32px 24px;
  }

  .link {
    color: #757575;
    font-weight: 400;
  }
}
</style>
