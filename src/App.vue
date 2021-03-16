<template>
  <div id="app">
    <router-view/>
  </div>
</template>
<script lang="ts">
import Vue from 'vue'
// import request from '@/utils/request'

export default Vue.extend({
  name: 'App'
})
</script>
<style lang="scss" scoped >
  h1 {
    color: $success-color;
  }
</style>
