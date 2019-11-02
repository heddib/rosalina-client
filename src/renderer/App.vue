<template>
  <div id="app">
    <title-bar :title='"Rosalina"'></title-bar>
    <router-view></router-view>
  </div>
</template>

<script>
  import TitleBar from '@/components/TitleBar'

  export default {
    name: 'rosalina',
    components: {
      TitleBar
    }
  }
</script>

<style>
  /* CSS */
  /* Disable select across the app */
  :not(input):not(textarea),
  :not(input):not(textarea)::after,
  :not(input):not(textarea)::before {
    -webkit-user-select: none;
    user-select: none;
    cursor: default;
  }
  input, button, textarea, :focus {
    outline: none;
  }
  img {
    -webkit-user-drag: none;
  }
</style>
