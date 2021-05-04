<template>
  <div class="layout px-4 py-4" style="max-width: 100%; display: block;">
    <Loading
      :isShow="showLoading"
    />
    <div class="layout__left" v-if="!showLoading">
      <h4>REMINDER (R1) {{ provider }}</h4>
      <div v-if="dataReminders.length > 0">
        <div class="user card" v-for="(item, id) in dataReminders" :key="id" style="margin-top: 15px">
          <div class="card-body">
            <TemplateWhatsapp
              :reminder="item"
            />
          </div>
        </div>
      </div>
      <div v-else>
        Untuk R1 {{ provider }} Hari ini tidak ada yang perlu di reminder
      </div>
    </div>
  </div>
</template>

<script>
import TemplateWhatsapp from '~/components/templates/whatsapp/template.vue'
import Loading from '~/components/mollecules/Loading'
import axios from 'axios'

export default {
  components: {
    TemplateWhatsapp,
    Loading
  },
  data() {
    return {
      REMINDER_URL: 'https://seakun-reminder-api.herokuapp.com',
      dataReminders: [],
      provider: '',
      showLoading: true
    }
  },
  mounted() {
    this.getReminderData()
  },
  methods: {
    async getReminderData() {
      const requestBody = {
        username: "seakun",
        password: "seakunatduaribu21"
      }
      this.provider = this.$route.query.provider
      try {
        const { data } = await axios.post(`${this.REMINDER_URL}/${this.provider.toLowerCase()}/r1`, requestBody)
        if (data) {
          this.dataReminders = data
        }
      } catch (err) {
        console.log(err);
      }
      this.showLoading = false
    }
  },
  watch: {
    $route () {
      this.showLoading = true
      this.dataReminders = []
      this.getReminderData()
    }
  }
}
</script>

<style lang="scss" scoped>
.user {
  padding: 16px;
}
</style>