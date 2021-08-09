<template>
  <div align="left" class="sidebar">
    <ul class="list-group">
      <li
        class="list-group-item"
        style="padding: 0px!important"
        v-for="(provider, id) in providers"
        :key="id"
      >
        <p class="label" @click.prevent="provider.isOpen = !provider.isOpen">
          {{ provider.name }}
        </p>
        <transition name="slide-fade">
          <ul class="sidebar__child" v-if="provider.isOpen">
            <!-- <li class="list-group-item child-list px-4" @click="$router.push(`/created-account?provider=${provider.name}`)">
                            Send Created {{ provider.name }} Account
                        </li>
                        <li class="list-group-item child-list px-4" @click="$router.push(`/billing?provider=${provider.name}`)">Send Billing</li> -->
            <li
              class="list-group-item child-list px-4"
              @click="$router.push(`/reminders/r1?provider=${provider.name}`)"
            >
              Reminder 1 (R1)
            </li>
            <li
              class="list-group-item child-list px-4"
              @click="$router.push(`/reminders/r2?provider=${provider.name}`)"
            >
              Expired Reminder (R2)
            </li>
          </ul>
        </transition>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      netflix: {
        isOpen: true,
        active: true
      },
      spotify: {
        isOpen: false,
        active: true
      },
      providers: [
        {
          name: "Netflix",
          isOpen: true,
          active: true
        },
        {
          name: "Spotify",
          isOpen: false,
          active: true
        },
        {
          name: "Gramedia",
          isOpen: false,
          active: true
        },
        {
          name: "Youtube",
          isOpen: false,
          active: true
        },
        {
          name: "Microsoft365",
          isOpen: false,
          active: true
        },
        {
          name: "Canva",
          isOpen: false,
          active: true
        },
        {
          name: "Disney+ Hotstar",
          isOpen: false,
          active: true
        },
        {
          name: "Apple One",
          isOpen: false,
          active: true
        },
        {
          name: "Wattpad",
          isOpen: false,
          active: true
        }
      ]
    };
  }
};
</script>

<style lang="scss" scoped>
.sidebar {
  margin-top: 52px;
  margin-left: 52px;
  .child-list {
    border: unset;
    padding: 9px 0px 9px 0px;
    &:hover {
      opacity: 0.7;
      background-color: #e5dafc;
      cursor: pointer;
    }
  }
  .list-group-item {
    cursor: pointer;
  }
  .label {
    padding: 16px;
    margin-bottom: 0px;
    &:hover {
      opacity: 0.7;
      background-color: #e5dafc;
      cursor: pointer;
    }
  }
  &__child {
    padding: 0px;
  }
  a {
    color: black;
    text-decoration: unset;
    padding: 0px 16px 0px 32px;
    &:hover {
      opacity: 0.7;
    }
  }
}
</style>
