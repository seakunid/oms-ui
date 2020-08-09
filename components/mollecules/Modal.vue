<template>
    <div v-if="openModal" class="modal fade show" id="modal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" >
        <div class="modal-dialog modal-dialog-scrollable">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="exampleModalLabel">{{title}}</h5>
                    <button @click="$emit('closeModal')" type="button" class="close" data-dismiss="modal" aria-label="Close">
                        <span>&times;</span>
                    </button>
                </div>
                <div class="modal-body scroller">
                    <slot></slot>
                </div>
                <div class="modal-footer" v-if="useFooter">
                    <button type="button" class="btn btn-cancel" data-dismiss="modal" @click="$emit('clickCancel')">{{secondaryBtn}}</button>
                    <button type="button" class="btn btn-secondary" @click="$emit('clickOk')">{{primaryBtn}}</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        openModal: Boolean,
        useFooter: {
            type: Boolean,
            default: false
        },
        title: {
            type: String,
            default: 'Modal'
        },
        secondaryBtn: {
            type: String,
            default: 'Batal'
        },
        primaryBtn: {
            type: String,
            default: 'Ok'
        }
    }
}
</script>

<style lang="scss" scoped>
.modal-dialog {
    max-width: 50rem;
}
.modal-body {
    padding: 0px!important;
}
.btn {
    min-width: 6rem;
}
@media (max-width: 800px) {
    .modal-dialog {
        max-width: auto!important;
    }
}
.show {
    display: block!important;
}
</style>