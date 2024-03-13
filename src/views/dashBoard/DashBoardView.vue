<!-- eslint-disable no-useless-escape -->
<template>
  <div class="containBox">
    <h1>這是後台頁</h1>
    <nav class="navbar navbar-expand-lg bg-body-tertiary">
      <div class="container-fluid">
        <a class="navbar-brand" href="#">Home</a>
        <button
          class="navbar-toggler"
          type="button"
          data-bs-toggle="collapse"
          data-bs-target="#navbarNav"
          aria-controls="navbarNav"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNav">
          <ul class="navbar-nav">
            <li class="nav-item">
              <RouterLink class="nav-link" to="/admin/productsControl"
                >Products Control Panel</RouterLink
              >
            </li>
            <li class="nav-item">
              <RouterLink class="nav-link" to="/admin/ordersControl"
                >Orders Control</RouterLink
              >
            </li>
            <li class="nav-item">
              <a href="#" class="nav-link" @click.prevent="signOut">登出</a>
            </li>
          </ul>
        </div>
      </div>
    </nav>
    <RouterView v-if="checkSuccess"></RouterView>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data () {
    return {
      checkSuccess: false,
      token: ''
    }
  },
  mounted () {
    this.checkLogin()
  },
  methods: {
    checkLogin () {
      const token = document.cookie.replace(
        /(?:(?:^|.*;\s*)hexToken\s*=\s*([^;]*).*$)|^.*$/,
        '$1'
      )
      if (token) {
        // Axios 預設值
        axios.defaults.headers.common.Authorization = `${token}`
        const api = `${import.meta.env.VITE_URL}api/user/check`
        axios
          .post(api, { api_token: this.token })
          .then(() => {
            this.checkSuccess = true
            // console.log(this.token)
          })
          .catch((err) => {
            alert(err.response.data.message)
            this.$router.push('/login')
          })
      } else {
        alert('您尚未登入。')
        this.$router.push('/login')
      }
    },
    signOut () {
      document.cookie = 'hexToken=;expires=;'
      alert('token 已清除')
      this.$router.push('/login')
    }
  }
}
</script>
