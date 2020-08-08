<template>
  <div>
        <div class="layout">
            <div class="layout__left">
                <SideBar/>
            </div>
            <div class="layout__right">
                <div class="content">
                    <form>
                        <div style="display: flex">
                          <div style="width: 50%; padding: 30px">
                            <div class="form-group">
                              <label for="name">Nama Lengkap User</label>
                              <input type="text" id="name" name="name" class="form-control" placeholder="Contoh: John Doe" @keydown="onChangeName" v-model="name">
                              <p class="error-msg" v-if="errorMsg.name">{{ errorMsg.name }}</p>
                            </div>
                            <div class="form-group">
                              <label for="email">Email User</label>
                              <input type="text" id="email" name="email" class="form-control" placeholder="Contoh: john@gmail.com" @keydown="onChangeEmail" v-model="email">
                              <p class="error-msg" v-if="errorMsg.email">{{ errorMsg.email }}</p>
                            </div>
                            <div class="form-group">
                              <label for="packet">Paket Yang dipilih User</label>
                              <input type="text" id="packet" name="packet" class="form-control" placeholder="Contoh: Paket Family (Group)" @keydown="onChangePacket" v-model="packet">
                              <p class="error-msg" v-if="errorMsg.packet">{{ errorMsg.packet }}</p>
                            </div>
                            <div class="form-group">
                              <label for="provider">Provider yang dipilih User</label>
                              <input type="text" id="provider" name="provider" class="form-control" placeholder="Contoh: Netflix" @keydown="onChangeProvider" v-model="provider">
                              <p class="error-msg" v-if="errorMsg.provider">{{ errorMsg.provider }}</p>
                            </div>
                          </div>
                          <div style="width: 50%; padding: 30px">
                            <div class="form-group">
                              <label for="username">Username {{ provider }}</label>
                              <input type="text" id="username" name="username" class="form-control" placeholder="Contoh: seakun.netflix@gmail.com" @keydown="onChangeUsername" v-model="username">
                              <p class="error-msg" v-if="errorMsg.username">{{ errorMsg.username }}</p>
                            </div>
                            <div class="form-group">
                              <label for="password">Password</label>
                              <input type="text" id="password" name="password" class="form-control" placeholder="Contoh: kuY#123ah" @keydown="onChangeUsername" v-model="password">
                              <p class="error-msg" v-if="errorMsg.password">{{ errorMsg.password }}</p>
                            </div>
                            <div class="form-group">
                              <label for="pin">Pin</label>
                              <input type="text" id="pin" name="pin" class="form-control" placeholder="Contoh: 1029" @keydown="onChangePin" v-model="pin">
                              <p class="error-msg" v-if="errorMsg.pin">{{ errorMsg.pin }}</p>
                            </div>
                          </div>
                        </div>

                        <button class="btn btn-primary" @click.prevent="clickSubmit" :disabled="isDisableBtn" style="min-width: 9rem; margin-left: 30px">
                          <span v-if="isDisableBtn" class="spinner-border spinner-border-sm text-dark" role="status" aria-hidden="true"></span>
                          Submit
                        </button>
                    </form>
                </div> 
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import SideBar from '~/components/mollecules/SideBar'
export default {
  components: {
    SideBar,
  },
  data() {
    return {
      name: '',
      email: '',
      packet: '',
      provider: '',
      username: '',
      password: '',
      pin: '',
      billing_date: '02 September 2020  ',
      errorMsg: {
        name: '',
        email: '',
        packet: '',
        provider: '',
        username: '',
        password: '',
        pin: '',
        billing_date: ''
      },
      isDisableBtn: false
    }
  },
  methods: {
    // validateInput() {
    //   !this.fullname ? this.errorMsg.fullname = 'Nama Lengkap harus diisi' : this.errorMsg.fullname = ''
    //   !this.email ? this.errorMsg.email = 'Email harus diisi' : this.errorMsg.email = ''
    //   !this.whatsapp ? this.errorMsg.whatsapp = 'Whatsapp harus diisi' : this.errorMsg.whatsapp = ''
    //   this.provider == 'Contoh: Netflix' ? this.errorMsg.provider = 'Provider Entertainment harus dipilih' : this.errorMsg.provider = ''
    //   this.packet == 'Contoh: Group (Family)' ? this.errorMsg.packet = 'Paket harus dipilih' : this.errorMsg.packet = ''
    // },
    // clickSubmitBray() {
    //   this.validateInput()
    //   if (
    //     this.fullname && 
    //     this.email && 
    //     this.whatsapp && 
    //     this.provider != 'Contoh: Netflix' && 
    //     this.packet != 'Contoh: Group (Family)'
    //   ) {
    //     this.isDisableBtn = true
    //     this.postRegisteredUser()
    //   }
    // },
    clickSubmit() {
      if (this.pin) {
        this.postRegisteredUser()
      } else {
        alert('pin isilah');
      }
    },
    postRegisteredUser() {
      let payload = {
        name: this.name,
        packet: this.packet,
        provider: this.provider,
        username: this.username,
        password: this.password,
        pin: this.pin,
        billing_date: this.billing_date
      }
      axios.post('http://localhost:4001/created-account', 
        payload,
        {
          rejectUnauthorized: false 
        }
      )
      .then(res => {
        console.log(res.data);
        console.log('Berhasil Kirim Akun ke Email User');
      })
      .catch(err => {
        console.log(err)
      })
    },
    onChangeName() {

    },
    onChangeEmail() {

    },
    onChangePacket() {

    },
    onChangeProvider() {

    },
    onChangeUsername() {

    },
    onChangePassword() {

    },
    onChangePin() {

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

.content {
  padding: 16px;
}

</style>
