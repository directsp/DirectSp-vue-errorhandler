<template>
    <transition name="modal">
        <div class="modal-mask" id="captcha">
            <div class="modal-wrapper">
                <div class="modal-container">
                    <div class="modal-header">
                        <slot name="header">
                            <h5>
                                {{$t('exceptionHandler.securityCheck')}}
                            </h5>
                        </slot>
                    </div>
                    <div class="modal-body">
                        <slot name="body">
                            <div class="form-group">
                                <div class="col-sm-12">
                                    {{$t('exceptionHandler.pleaseEnterCaptcha')}}
                                </div>
                            </div>
                            <div class="form-group">
                                <div class="col-sm-12 ">
                                    <img :src="handler.captchaImageUri">
                                </div>
                            </div>
                            <div class="form-group">
                                <div class="col-sm-12">
                                    <input v-focus :name="$t('exceptionHandler.captcha')" type="number" class="form-control" v-model="captcha" v-validate="'required'" :class="{'input': true,'is-danger': veeErrors.has($t('exceptionHandler.captcha')) }" min="0" inputmode="numeric" pattern="[0-9]*"
                                    :placeholder="$t('exceptionHandler.type')">
                                    <span v-if="veeErrors.has($t('exceptionHandler.captcha'))" class="help is-danger">{{ veeErrors.first($t('exceptionHandler.captcha')) }}</span>
                                </div>
                            </div>
                        </slot>
                    </div>
                    <div class="modal-footer">
                        <slot name="footer">
                            <div>
                                <button type="button" class="verify btn" @click="submit">
                                    <span class="btn__content">
                                        <i class="material-icons icon verify-icon">check</i>
                                        {{$t('exceptionHandler.verify')}}
                                    </span>
                                </button>
                                <button type="button" class="cancel btn" @click="cancel">
                                    <span class="btn__content">
                                        <i class="material-icons icon cancel-icon">close</i>
                                        {{$t('exceptionHandler.cancel')}}
                                    </span>
                                </button>
                            </div>
                        </slot>
                    </div>
                </div>
            </div>
        </div>
    </transition>
</template>

<script>
export default {
    name: 'captcha',
    props: {
        handler: Object,
    },
    data() {
        return {
            captcha: null,
        }
    },
    created() {
    },
    mounted() {
        document.getElementById('captcha').addEventListener("keydown", this.checkKey);
    },
    beforeDestroy: function() {
        document.getElementById('captcha').removeEventListener('keydown', this.checkKey);
    },
    methods: {
        submit() {
            let isValid = this.checkBusinessRules();
            if (isValid) {
                this.handler.captchaCode = this.captcha;
                this.handler.retry();
                this.$emit('close');
            }
        },
        cancel() {
            this.handler.release();
            this.$emit('close');
        },
        checkBusinessRules() {
            let isValid = true;
            this.$validator.validateAll();
            if (this.veeErrors.any()) {
                isValid = false;
            }
            return isValid;
        },
        checkKey: function(e) {
            if (e.key == "Enter") {
                e.preventDefault();
                this.submit();
            }

            if (e.key == 'Escape') {
                e.preventDefault();
                this.cancel;
            }
        },
    }
}
</script>

<style>

</style>
