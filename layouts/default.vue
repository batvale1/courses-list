<template>
  <div class="root">
    <app-header />
    <Nuxt />
  </div>
</template>

<script>
import Header from '@/components/shared/Header';
export default {
  components: {
    'app-header': Header,
  },
  async created() {
    await this.$store.dispatch('data-list/getList');
  },
};
</script>
<style>
html {
  font-family: 'Source Sans Pro', -apple-system, BlinkMacSystemFont, 'Segoe UI',
    Roboto, 'Helvetica Neue', Arial, sans-serif;
  font-size: 16px;
  word-spacing: 1px;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
  box-sizing: border-box;
}

*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
}

.root {
  background-color: black;
  min-height: 100vh;
  font-family: 'Inter';
  padding-bottom: 194px;
  -webkit-font-smoothing: antialiased;
}
</style>
