<template>
  <section class="app-main">
    <transition name="fade-transform" mode="out-in">
      <!--      or name="fade"-->
      <!--      <keep-alive :include="cachedViews">-->
      <!--        <router-view :key="key" />-->
      <!--      </keep-alive>-->
      <router-view :key="key" />
    </transition>
  </section>
</template>

<script>
export default {
  name: 'AppMain',
  computed: {
    key() {
      return this.$route.name !== undefined ? this.$route.name + +new Date() : this.$route + +new Date()
    }
  }
}
</script>

<style scoped>
.app-main {
  /*50 = navbar  */
  min-height: calc(100vh - 50px);
  position: relative;
  overflow: hidden;
}
</style>
