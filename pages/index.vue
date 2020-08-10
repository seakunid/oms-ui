<template>
  <div>
    <div v-if="isLoggedIn">
      <HomepageAfterLogin/>
    </div>
    <div v-else>
      <div class="container">
        <form>
          <input v-model="username" type="text" id="username" name="username" class="form-control" placeholder="username" style="margin: 16px 0px">
          <input v-model="password" type="password" id="password" name="password" class="form-control" placeholder="password" style="margin: 16px 0px">
          <p v-if="errorMsg">{{errorMsg}}</p>
          <button class="btn btn-primary" @click.prevent="clickLogin" :disabled="isDisableBtn" style="min-width: 6rem;">
            <span v-if="isDisableBtn" class="spinner-border spinner-border-sm text-dark" role="status" aria-hidden="true"></span>
            Masuk
          </button>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import HomepageAfterLogin from '~/components/pages/homepage-after-login'
export default {
  components: {
    HomepageAfterLogin
  },
  data() {
    return {
      adminData: [
        {
          username: 'aadilah@seakun.id',
          password: 'seakun#123'
        },
        {
          username: 'faishal@seakun.id',
          password: 'seakun#123'
        }
      ],
      username: '',
      password:'',
      errorMsg: '',
      isDisableBtn: false,
      isLoggedIn: false
    }
  },
  beforeMount() {
    this.checkLoggedUser()
  },
  methods: {
    checkLoggedUser() {
      const expired = localStorage.getItem('expired') && localStorage.getItem('expired')
      if (expired) {
        Date.now() < expired ? this.isLoggedIn = true : this.isLoggedIn = false
      } else {
        this.isLoggedIn = false
      }
    },
    clickLogin() {
      if (!this.username || !this.password) {
        this.errorMsg = 'Username atau Password wajib diisi'
      } else {
        this.errorMsg = ''
        let arr = []
        this.adminData.map(data => {
          if (data.username == this.username && data.password == this.password) {
            arr.push(1)
          } else {
            arr.push(0)
          }
        })
        arr.sort().reverse()
        if (arr[0] == 1) {
          const fiveTeenMinutes = 900000
          let expired = Date.now() + fiveTeenMinutes
          localStorage.setItem('expired', expired)
          this.isLoggedIn = true
        } else {
          this.errorMsg = 'Username atau password salah'
        }
      }
    }
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

.container {
  margin: 0 auto;
  min-height: 85vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.content {
  padding: 16px;
}

.title {
  font-family:
    'Quicksand',
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>