<template>
  <div class="created-account">
    <Modal 
      :openModal="openModal" 
      @closeModal="openModal = false" 
      title="Preview Email" 
      :useFooter="true" 
      primaryBtn="Kirim"
      @clickOk="clickOkModal"
      @clickCancel="openModal = false"
    >
      <CreatedAccount
        :name="name"
        :packet="packet"
        :provider="provider"
        :username="username"
        :password="password"
        :pin="pin"
        :billingDate="billing_date"
      />
    </Modal>
    <transition name="slide-fade">
      <div v-if="showSnackBar" id="snackbar">
        <span style="text-align: end;" @click="showSnackBar = false">&times;</span>
        <p>Berhasil kirim email ke User, Thankyou!</p>
      </div>
    </transition>
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
                  <ButtonDrop
                    @onClick="showPackets = !showPackets"
                    label="Paket yang dipilih User"
                    :btnText="packet"
                  />
                  <p class="error-msg" v-if="errorMsg.packet">{{ errorMsg.packet }}</p>
                </div>
                <transition name="slide-fade">
                  <div class="dropdown" v-if="showPackets">
                    <div v-for="(packet, id) in packets" :key="id" class="dropdown__item" @click="choosePacket(packet)">
                      {{ packet.name }} 
                    </div>
                  </div>
                </transition>
              </div>
              <div style="width: 50%; padding: 30px">
                <div class="form-group">
                  <label for="username">Username {{ $route.query.provider }}</label>
                  <input type="text" id="username" name="username" class="form-control" placeholder="Contoh: seakun.usermail@gmail.com" @keydown="onChangeUsername" v-model="username">
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
                <p class="button-referral-code" @click="useReferralCode = !useReferralCode">Use Referral Code</p>
                <div class="form-group" v-if="useReferralCode">
                  <label for="referral-code">Referral Code</label>
                  <input type="text" id="referral-code" name="referral-code" class="form-control" placeholder="Contoh: seakunid" v-model="referal_code">
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
import ButtonDrop from '~/components/atoms/ButtonDropDown'
import CreatedAccount from '~/components/templates/mail/created-account'
import Modal from '~/components/mollecules/Modal'
import moment from 'moment'
export default {
  components: {
    SideBar,
    ButtonDrop,
    CreatedAccount,
    Modal
  },
  data() {
    return {
      name: '',
      email: '',
      packet: 'Contoh: Paket Premium Grup',
      provider: this.$route.query.provider,
      username: '',
      password: '',
      pin: '',
      billing_date: '',
      referal_code: '',
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
      showPackets: false,
      packets: [
        {name: 'Paket Premium Grup (USER REGULER)'},
        {name: 'Paket Premium Grup (USER HOST)'},
      ],
      isDisableBtn: false,
      openModal: false,
      showSnackBar: false,
      useReferralCode: false
    }
  },
  created() {
    moment.locale('id')
  },
  mounted() {
    this.setBillingDate()
  },
  beforeMount() {
    this.checkLoggedUser()
  },
  methods: {
    checkLoggedUser() {
      const expired = localStorage.getItem('expired') && localStorage.getItem('expired')
      if (expired) {
        Date.now() >= expired && (window.location = '/')
      } else {
        window.location = '/'
      }
    },
    validateInput() {
      !this.name ? this.errorMsg.name = 'Nama Lengkap User harus diisi' : this.errorMsg.name = ''
      !this.email ? this.errorMsg.email = 'Email User harus diisi' : this.errorMsg.email = ''
      this.packet == 'Contoh: Paket Premium Grup' ? this.errorMsg.packet = 'Paket harus dipilih' : this.errorMsg.packet = ''
      !this.username ? this.errorMsg.username = `Username ${this.provider} harus diisi` : this.errorMsg.username = ''
      !this.password ? this.errorMsg.password = `Password ${this.provider} harus diisi` : this.errorMsg.password = ''
      !this.pin ? this.errorMsg.pin = `Pin ${this.provider} harus diisi` : this.errorMsg.pin = ''
    },
    clickSubmit() {
      this.validateInput()
      if (
        this.name && 
        this.email && 
        this.packet != 'Contoh: Paket Premium Grup' && 
        this.username &&
        this.password &&
        this.pin
      ) {
        this.openModal = true
      }
    },
    clickOkModal() {
      this.postRegisteredUser()
      this.isDisableBtn = true
      this.openModal = false
    },
    postRegisteredUser() {
      let payload = {
        name: this.name,
        email: this.email,
        packet: this.packet,
        provider: this.provider,
        username: this.username,
        password: this.password,
        pin: this.pin,
        billing_date: this.billing_date,
        referal_code: this.referal_code
      }
      axios.post('https://seakun-mail-api-v1.herokuapp.com/created-account', payload)
      .then(res => {
        if (res) {
          this.isDisableBtn = false
          this.showSnackBar = true
          console.log('Berhasil Kirim Akun ke Email User')
          this.resetValue()
        }
      })
      .catch(err => {
        console.log(err)
      })
    },
    setBillingDate() {
      const billingDate = moment().add('days', 24).calendar()
      this.billing_date = moment(billingDate, 'DDMMYY').format('LL')
    },
    choosePacket(packet) {
      this.packet = packet.name
      this.showPackets = false
      this.errorMsg.packet = ''
    },
    resetValue() {
      this.name = ''
      this.email = ''
      this.packet = 'Contoh: Paket Premium Grup'
      this.username = ''
      this.password = ''
      this.pin = ''
      this.referal_code = ''
    },
    onChangeName() {
      this.errorMsg.name = ''
    },
    onChangeEmail() {
      this.errorMsg.email = ''
    },
    onChangePacket() {
      this.errorMsg.packet = ''
    },
    onChangeUsername() {
      this.errorMsg.username = ''
    },
    onChangePassword() {
      this.errorMsg.password = ''
    },
    onChangePin() {
      this.errorMsg.pin = ''
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

.created-account {
  .dropdown {
    border-radius: .25rem;
    border: 1px solid #ced4da;
    padding: 0px;
    margin-bottom: 20px;
    max-width: 100%;
    &__item {
      padding: 8px 16px!important;
      display: flex;
      align-items: center;
      justify-content: space-between;
      cursor: pointer;
      &:hover {
        background-color: #eeeeee;
      }
    }
  }
  .error-msg {
    color: red;
    font-size: 12px;
  }
  #snackbar {
    background-color: #daeeef;
    color: #2f524b;
    text-align: center;
    border-radius: 2px;
    padding: 16px;
    position: fixed;
    z-index: 1;
    top: 130px;
    font-size: 17px;
    margin: 0 auto;
    max-width: 600px;
    left: 50%;
    margin-left: -300px;
    font-weight: 400;
    display: grid;
    button {
      margin-top: 0px!important;
      margin-bottom: 10px!important;
    }
    span {
      font-size: 28px;
      font-weight: 700;
      cursor: pointer;
      padding: 0px 12px;
    }
  }
  .button-referral-code {
    color: blue;
    cursor: pointer;
  }
}
</style>
