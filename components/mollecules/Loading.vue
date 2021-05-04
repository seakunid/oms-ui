<template>
  <div class="loading" v-if="isShow">
    <div class="spinner-grow text-info" role="status">
      <span class="visually-hidden"></span>
    </div>
    <div class="spinner-grow text-info" role="status">
      <span class="visually-hidden"></span>
    </div>
    <div class="spinner-grow text-info" role="status">
      <span class="visually-hidden"></span>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    isShow: {
      type: Boolean,
      default: false
    }
  }
}
</script>

<style lang="scss">
.loading {
  text-align: center;
  padding: 100px 0px;
}
</style>