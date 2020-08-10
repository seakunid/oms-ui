<template>
    <div align="left" class="sidebar">
        <ul class="list-group">
            <li class="list-group-item" style="padding: 0px!important">
                <p class="label" @click.prevent="netflix.isOpen = !netflix.isOpen">Netflix</p>
                <transition name="slide-fade">
                    <ul class="sidebar__child" v-if="netflix.isOpen">
                        <li class="list-group-item child-list" @click="$router.push('/created-account?provider=Netflix')">
                            <a>Send Created Netflix Account</a>
                        </li>
                        <li class="list-group-item child-list" @click="$router.push('/billing?provider=Netflix')">
                            <a>Send Billing</a>
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
        padding: 9px 0px 9px 0px;
        &:hover {
            opacity: .7;
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
            opacity: .7;
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
            opacity: .7;
        }
    }
}
</style>
