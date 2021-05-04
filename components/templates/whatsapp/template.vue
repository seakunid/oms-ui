<template>
  <div v-if="reminder">
    <div class="template-redirect">
      <a :href="`https://wa.me/${reminder.whatsapp}?text=${encodeURI(reminder.message)}`" target="_blank">Redirect to whatsapp {{ reminder.name }} {{ reminder.whatsapp }}</a>
      <button class="btn btn-secondary px-3 mb-3" @click="handleCopyTemplate(reminder)">Copy Template</button>
    </div>
    <div style="text-align: center"><textarea name="" id="" cols="106" rows="8" :value="reminder.message" disabled></textarea></div>

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
export default {
  props: {
    reminder: {
      type: Object,
      default: {
        name: "Udin",
        email: "Udin Penyok@mail",
        whatsapp: "628293948324",
        message: "",
      }
    },
  },
  data() {
    return {
      showSnackBar: false,
    }
  },
  methods: {
    handleCopyTemplate(reminder) {
      const elementText = reminder.message

      navigator.clipboard.writeText(elementText).then(
        () => {
          this.showSnackBar = true;
          setTimeout(() => {
            this.showSnackBar = false;
          }, 2000);
        },
        (err) => console.log(err)
      );
    }
  },
}
</script>

<style lang="scss" scoped>
.template {
  &-redirect {
    display: flex;
    justify-content: space-between;
  }
}
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
</style>