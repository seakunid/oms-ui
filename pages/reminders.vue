<template>
  <div>
    <div class="layout">
      <div class="layout__left">
        <SideBar/>
      </div>
      <div class="layout__right">
        <nuxt-child/>
      </div>
    </div>
  </div>
</template>

<script>
import SideBar from '~/components/mollecules/SideBar'
export default {
    components: {
        SideBar,
    }
}
</script>

<style lang="scss" scoped>
.layout {
  display: flex;
  &__left {
    width: 26%;
  }
  &__right {
    width: 74%;
  }
}

@media (max-width: 800px) {
  .layout {
    &__left {
      width: 39%;
    }
  }
}

</style>
