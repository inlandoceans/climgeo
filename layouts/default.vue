<template>
  <div class="container mx-auto my-4">
    <TheHeader />
    <Nuxt />
  </div>
</template>

<style>
html {
  font-family: 'Crimson Text', 'Source Sans Pro', -apple-system,
    BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
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
</style>

<script>
export default {
  head: {
    title: 'Climate Geopolitics',
    meta: [
      { charset: 'utf-8' },
      { name: 'viewport', content: 'width=device-width, initial-scale=1' },
      {
        hid: 'description',
        name: 'description',
        content: ''
      }
    ],
    link: [
      {
        rel: 'stylesheet',
        href:
          'https://fonts.googleapis.com/css2?family=Spectral:wght@400;500;600&display=swap'
      },
      {
        rel: 'stylesheet',
        href:
          'https://fonts.googleapis.com/css2?family=Crimson+Text:wght@700&family=Montserrat:wght@500&display=swap'
      }
    ]
  }
}
</script>

<style scoped>
/* Page Transitions - 0.4s Slide/Fade */
.page-enter-active,
.page-leave-active {
  transition-duration: 0.4s;
  transition-property: height, opacity, transform;
  transition-timing-function: cubic-bezier(0.55, 0, 0.1, 1);
  overflow: hidden;
}

.page-enter {
  opacity: 0;
  transform: translate(2em, 0);
}

.page-leave-active {
  opacity: 0;
  transform: translate(-2em, 0);
}
</style>
