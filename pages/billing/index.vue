<template>
  <div>
        <div class="layout">
            <div class="layout__left">
                <SideBar/>
            </div>
            <div class="layout__right">
                <div class="content">
                    Billing
                </div> 
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

.content {
  padding: 16px;
}

</style>
