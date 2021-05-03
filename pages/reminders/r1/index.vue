<template>
  <div class="layout px-4 py-4" style="max-width: 50%">
    <div class="layout__left">
      <div class="user card" v-for="(user, id) in users" :key="id" style="margin-top: 15px">
        <div class="card-body">
          <TemplateWhatsappNetflix
            :user="user"
          />
        </div>
      </div>
    </div>

    <transition name="slide-fade">
      <div v-if="showSnackBar" id="snackbar">
        <span style="text-align: end" @click="showSnackBar = false"
          >&times;</span
        >
        <p>Template disalin</p>
      </div>
    </transition>
  </div>
</template>

<script>
import TemplateWhatsappNetflix from '~/components/templates/whatsapp/netflix.vue'

export default {
  components: {
    TemplateWhatsappNetflix
  },
  data() {
    return {
      users: [
        {
          name: "Faishal Arif",
          email: "faishal.arif218@gmail.com",
          whatsapp: "6282277068394",
          packet: "Paket Premium Grup (USER REGULER)",
          price: "Rp53.000",
          reminderDate: "3 Mei 2021",
          expiredDate: "10 Mei 2021",
        },
        {
          name: "ucok Arif",
          email: "ucok.arif218@gmail.com",
          whatsapp: "6282277068394",
          packet: "Paket Premium Grup (USER REGULER)",
          price: "Rp53.000",
          reminderDate: "3 Mei 2021",
          expiredDate: "10 Mei 2021",
        }
      ],
      showSnackBar: false,
    }
  },
  methods: {
    
  }

}
</script>

<style lang="scss" scoped>
#snackbar {
  background-color: #daeeef;
  color: #2f524b;
  text-align: center;
  border-radius: 4px;
  padding: 16px;
  position: fixed;
  z-index: 1;
  top: 100px;
  font-size: 17px;
  margin: 0 auto;
  max-width: 600px;
  left: 65%;
  margin-left: -300px;
  font-weight: 400;
  display: grid;
  button {
    margin-top: 0px !important;
    margin-bottom: 10px !important;
  }
  span {
    font-size: 28px;
    font-weight: 700;
    cursor: pointer;
    padding: 0px 12px;
  }
}
@media (max-width: 800px) {
  #snackbar {
    position: absolute !important;
    max-width: 200px !important;
    left: 30% !important;
    top: 60% !important;
    margin-left: 0px !important;
  }
}
.user {
  padding: 16px;
}
</style>