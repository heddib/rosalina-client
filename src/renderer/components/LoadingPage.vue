<template>
  <div class="center">
    <!-- <img id="photoshy" src="~@/assets/white-text.png" alt="electron-vue"> -->
    <video autoplay loop id="photoshy" width="250" height="250">
      <source src="@/assets/photoshy.webm" type="video/webm">
    </video>
    <div class="loading">Chargement...</div>
  </div>
</template>

<script>
  const Store = require('electron-store')
  const store = new Store({name: 'settings'})
  const uuid = require('uuid/v4')
  export default {
    name: 'loading-page',
    methods: {
    },
    mounted () {
      if (store.size === 0) {
        console.log('New')
        store.set('uuid', uuid())
      } else {
        console.log('Pas New')
      }
    }
  }
</script>

<style>
  @import url('https://fonts.googleapis.com/css?family=Source+Sans+Pro');

  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  body { font-family: 'Source Sans Pro', sans-serif; background-color: #2C2F33; }

  .center {
    text-align: center;
  }

  #photoshy {
    margin-bottom: 20px;
    margin-top: 150px;
  }

  .loading {
    font-size: 16pt;
    color: white;
  }

</style>
