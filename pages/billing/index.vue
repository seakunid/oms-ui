<template>
  <div class="billing">
    <Modal 
      :openModal="openModal" 
      @closeModal="openModal = false" 
      title="Preview Email" 
      :useFooter="true" 
      primaryBtn="Kirim"
      @clickOk="clickOkModal"
      @clickCancel="openModal = false"
    >
      <BillingMonthly
        :fullname="fullname"
        :packet="packet"
        :provider="provider"
        :price="`Rp ${price.toString().replace(/(\d)(?=(\d{3})+(?!\d))/g, '$1.')}`"
        :lastActiveDate="returnValueDate(lastActiveDate)"
      />
    </Modal>
    <transition name="slide-fade">
      <div v-if="showSnackBar" id="snackbar">
        <span style="text-align: end;" @click="showSnackBar = false">&times;</span>
        <p>Berhasil kirim Email <b>Billing</b> ke User, Thankyou!</p>
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
                  <label for="fullname">Nama Lengkap User</label>
                  <input type="text" id="fullname" name="fullname" class="form-control" placeholder="Contoh: John Doe" @keydown="onChangeFullame" v-model="fullname">
                  <p class="error-msg" v-if="errorMsg.fullname">{{ errorMsg.fullname }}</p>
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
                  <label for="price">Masukkan Biaya yang harus dibayar User</label>
                  <input type="text" id="price" name="price" class="form-control" placeholder="Contoh: 49000" @keydown="onChangePrice" v-model="price">
                  <p class="error-msg" v-if="errorMsg.price">{{ errorMsg.price }}</p>
                </div>
                <div class="form-group">
                  <label for="lastActiveDate">Tanggal Terakhir Aktif Layanan</label>
                  <input type="text" id="lastActiveDate" name="lastActiveDate" class="form-control" placeholder="DD/MM/YYYY" @keydown="onChangelastActiveDate" v-model="lastActiveDate">
                  <p class="error-msg" v-if="errorMsg.lastActiveDate">{{ errorMsg.lastActiveDate }}</p>
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
import BillingMonthly from '~/components/templates/mail/billing-monthly'
import Modal from '~/components/mollecules/Modal'
import moment from 'moment'
export default {
  components: {
    SideBar,
    ButtonDrop,
    BillingMonthly,
    Modal
  },
  data() {
    return {
      fullname: '',
      email: '',
      packet: 'Contoh: Paket Premium Group (Family)',
      provider: this.$route.query.provider,
      price: '',
      lastActiveDate: '',
      errorMsg: {
        fullname: '',
        email: '',
        packet: '',
        provider: '',
        price: '',
        lastActiveDate: ''
      },
      showPackets: false,
      packets: [
        {name: 'Paket Premium Group (Family)'},
        {name: 'Paket Mobile Personal'},
        {name: 'Paket Basic Personal'},
        {name: 'Paket Standar Personal'}
      ],
      isDisableBtn: false,
      openModal: false,
      showSnackBar: false
    }
  },
  created() {
    moment.locale('id')
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
      !this.fullname ? this.errorMsg.fullname = 'Nama Lengkap User harus diisi' : this.errorMsg.fullname = ''
      !this.email ? this.errorMsg.email = 'Email User harus diisi' : this.errorMsg.email = ''
      this.packet == 'Contoh: Paket Premium Group (Family)' ? this.errorMsg.packet = 'Paket harus dipilih' : this.errorMsg.packet = ''
      !this.price ? this.errorMsg.price = `Biaya harus diisi` : this.errorMsg.price = ''
      !this.lastActiveDate ? this.errorMsg.lastActiveDate = 'Tanggal Terakhir Aktif layanan harus diisi' : this.errorMsg.lastActiveDate = ''
    },
    clickSubmit() {
      this.validateInput()
      if (
        this.fullname && 
        this.email && 
        this.packet != 'Contoh: Paket Premium Group (Family)' && 
        this.price &&
        this.lastActiveDate
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
        fullname: this.fullname,
        email: this.email,
        packet: this.packet,
        provider: this.provider,
        price: `Rp${this.price.toString().replace(/(\d)(?=(\d{3})+(?!\d))/g, '$1.')}`,
        last_active_date: this.returnValueDate(this.lastActiveDate)
      }
      // axios.post('https://seakun-mail-api.herokuapp.com/billing', payload)
      axios.post('http://localhost:4001/billing', payload)
      .then(res => {
        if (res) {
          this.isDisableBtn = false
          this.showSnackBar = true
          console.log('Berhasil Kirim Billing ke Email User')
          this.resetValue()
        }
      })
      .catch(err => {
        console.log(err)
      })
    },
    choosePacket(packet) {
      this.packet = packet.name
      this.showPackets = false
      this.errorMsg.packet = ''
    },
    resetValue() {
      this.fullname = ''
      this.email = ''
      this.packet = 'Contoh: Paket Premium Group (Family)'
      this.price = ''
      this.lastActiveDate = ''
    },
    onChangeFullame() {
      this.errorMsg.fullname = ''
    },
    onChangeEmail() {
      this.errorMsg.email = ''
    },
    onChangePacket() {
      this.errorMsg.packet = ''
    },
    onChangePrice() {
      this.errorMsg.price = ''
    },
    onChangelastActiveDate() {
      this.errorMsg.lastActiveDate = ''
    },
    returnValueDate(val) {
      return `${moment(val, 'DDMMYYYY').format('LL')}`
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

.billing {
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
}
</style>
