<template>
    <div class="error-handler-container">
        <error-message v-if="showErrorMessage" :showRetryBtn="showRetryBtn" :handler="handler" :translateErrorName="translateErrorName" @close="showErrorMessage = false"></error-message>
        <captcha v-if="showCaptcha" :handler="handler" @close="showCaptcha = false"></captcha>
    </div>
</template>

<script>
import Captcha from './Captcha.vue'
import ErrorMessageModal from './ErrorMessage.vue'

export default {
    name: 'error-handler',
    props: {
        translateErrorName: Function,
    },
    components: {
        'captcha': Captcha,
        'error-message': ErrorMessageModal,
    },
    data() {
        return {
            showCaptcha: false,
            handler: {},
            showErrorMessage: false,
            showRetryBtn: false,
        }
    },
    created() {
        this.$bus.$on('handleError', this.handleError);
    },
    updated() {
    },
    methods: {
        handleError(handler) {
            this.handler = handler;
            if (handler.error.errorName == 'InvalidCaptcha') {
                this.showCaptcha = true;
            }
            else {
                this.showRetryBtn = handler.canRetry? true : false;
                this.showErrorMessage = true;
            }
        }
    },
}
</script>


