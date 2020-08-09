<template>
    <div align="left" class="sidebar">
        <ul class="list-group">
            <li class="list-group-item">
                <p class="label" @click.prevent="netflix.isOpen = !netflix.isOpen">Netflix</p>
                <transition name="slide-fade">
                    <ul class="sidebar__child" v-if="netflix.isOpen">
                        <li class="list-group-item child-list">
                            <nuxt-link to="/created-account?provider=Netflix">Send Created Netflix Account</nuxt-link>
                        </li>
                        <li class="list-group-item child-list">
                            <nuxt-link to="/billing?provider=Netflix">Send Billing</nuxt-link>
                        </li>
                    </ul>
                </transition>
            </li>
            <!-- <li class="list-group-item">
                <p class="label" @click.prevent="spotify.isOpen = !spotify.isOpen">Spotify</p>
                <transition name="slide-fade">
                    <ul class="sidebar__child" v-if="spotify.isOpen">
                        <li class="list-group-item child-list">
                            <nuxt-link to="/created-account?provider=Spotify">Send Created Spotify Account</nuxt-link>
                        </li>
                        <li class="list-group-item child-list">
                            <nuxt-link to="/billing?provider=Spotify">Send Billing</nuxt-link>
                        </li>
                    </ul>
                </transition>
            </li> -->
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
        }
    }
}
</script>

<style lang="scss" scoped>
.sidebar {
    margin-top: 52px;
    margin-left: 52px;
    .child-list {
        border: unset;
        padding: 0px 0px 5px 0px;
        &:hover {
            color: #b1a2d1;
            cursor: pointer;
        }
    }
    .list-group-item {
        cursor: pointer;
    }
    .label {
        margin-bottom: 10px;
        &:hover {
            color: #b1a2d1;
            cursor: pointer;
        }
    }
    &__child {
        padding: 10px 0px 10px 16px;
    }
}
</style>
