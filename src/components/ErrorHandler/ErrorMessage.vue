<template>
    <transition name="modal">
        <div class="modal-mask" id="error-message-modal">
            <div class="modal-wrapper">
                <div class="modal-container">
                    <div class="modal-header">
                        <h3>
                            {{$t('exceptionHandler.error')}}
                        </h3>
                        <i class="material-icons icon" @click="cancel">close</i>
                    </div>
                    <div class="modal-body">
                        <slot name="body">
                            <div class="table-responsive  error-advance" v-if="handler.error.errorNumber != 503 && handler.error.errorNumber != 55027 && handler.error.errorNumber != 55028">
                                <table class="table table-bordered">
                                    <tr>
                                        <td>
                                             <b>Error Number</b>
                                        </td>
                                        <td>
                                            {{handler.error.errorNumber}}
                                        </td>
                                    </tr>
                                    <tr>
                                        <td>
                                            <b>Error Type</b>
                                        </td>
                                        <td>
                                            {{handler.error.errorType}}
                                        </td>
                                    </tr>
                                    <tr>
                                        <td>
                                            <b>Error Name</b>
                                        </td>
                                        <td>
                                            {{translatedErrorName}}
                                        </td>
                                    </tr>
                                    <tr>
                                        <td>
                                           <b>Error Message</b>
                                        </td>
                                        <td>
                                            {{handler.error.errorMessage}}
                                        </td>
                                    </tr>
                                    <tr>
                                        <td>
                                            <b>Error Description</b>
                                        </td>
                                        <td>
                                           {{handler.error.errorDescription}}
                                        </td>
                                    </tr>
                                    <tr>
                                        <td>
                                            <b>Status</b>
                                        </td>
                                        <td>
                                            {{handler.error.status}}
                                        </td>
                                    </tr>
                                    <tr>
                                        <td>
                                            <b>Status Text</b>
                                        </td>
                                        <td>
                                            {{handler.error.statusText}}
                                        </td>
                                    </tr>
                                </table>
                            </div>
                            <div class="error-simple" v-else>
                                <img v-if="handler.error.errorNumber == 503" class="error-simple-pic" src="/src/assets/modules/directsp-errorhandler-vue/Img/network-error.png" />
                                <img v-if="handler.error.errorNumber == 55027" class="error-simple-pic" src="/src/assets/modules/directsp-errorhandler-vue/Img/understruction-error.png" />
                                <img v-if="handler.error.errorNumber == 55028" class="error-simple-pic" src="/src/assets/modules/directsp-errorhandler-vue/Img/lock-error.png" />
                                <p :class="{'error-simple-text503' : handler.error.errorNumber == 503, 'error-simple-text55027' : handler.error.errorNumber == 55027, 'error-simple-text55028' : handler.error.errorNumber == 55028}">
                                    {{translatedErrorName}}
                                </p>
                            </div>
                        </slot>
                    </div>
                    <div class="modal-footer">
                        <slot name="footer">
                            <button v-if="showRetryBtn" type="button" class="verify btn" @click="retry">
                                <span class="btn__content">
                                    <i class="material-icons icon verify-icon">check</i>
                                    {{$t('exceptionHandler.retry')}}
                                </span>
                            </button>
                            <button type="button" class="cancel btn" @click="cancel">
                                <span class="btn__content">
                                    <i class="material-icons icon cancel-icon">close</i>
                                    {{$t('exceptionHandler.cancel')}}
                                </span>
                            </button>
                        </slot>
                    </div>
                </div>
            </div>
        </div>
    </transition>
</template>

<script>
export default {
    name: 'error-message-modal',
    props: {
        translateErrorName: Function,
        handler: Object,
        showRetryBtn: Boolean,
    },
    computed: {
        translatedErrorName: function () {
            if (this.translateErrorName) {
                return this.translateErrorName(this.handler.error);
            }
            else {
                return this.handler.error.errorName;
            }
        }
    },
    mounted() {
        document.getElementById('error-message-modal').addEventListener("keydown", this.checkKey);
    },
    beforeDestroy: function() {
        document.getElementById('error-message-modal').removeEventListener('keydown', this.checkKey);
    },
    methods: {
        retry() {
            this.handler.retry();
            this.$emit('close');
        },
        cancel() {
            this.handler.release();
            this.$emit('close');
        },
        checkKey: function(e) {
            if (e.key == "Enter") {
                e.preventDefault();
                this.retry();
            }

            if (e.key == 'Escape') {
                e.preventDefault();
                this.cancel;
            }
        },
    },
}
</script>

<style>

</style>


