<template>
  <div class="d-flex align-items-center justify-content-center vh-100">
    <div class="text-center">
      <h1 class="display-1 fw-bold">{{ title }}</h1>
      <routerLink to="/">To Home Page</routerLink>
    </div>
  </div>
</template>

<script>
export default {
  name: 'NotFound',
  components: {},
  data () {
    return {
      title: '404'
    }
  }
}
</script>

<style lang="scss" scoped>
</style>
