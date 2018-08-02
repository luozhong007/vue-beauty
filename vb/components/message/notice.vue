<template>
    <div class="ant-message-notice" transition="message">
        <div class="ant-message-notice-content">
            <div :class="[`${prefixCls}-custom-content ${prefixCls}-${type}`]">
                <i v-show="type" :class="iconClasses"></i>
                <span v-text="content"></span>
            </div>
        </div>
    </div>
</template>
<script>

export default {
    props: {
        content: {
            type: String,
        },
        duration: {
            type: Number,
        },
        type: {
            type: String,
        },
        selfKey: {
            type: String,
            required: true,
        },
        onClose: {
            type: Function,
            default() { },
        },
    },
    data() {
        return {
            prefixCls: 'ant-message',
            spin: false,
            prefix: 'anticon',
        };
    },
    computed: {
        iconType() {
            return ({
                info: 'info-circle',
                success: 'check-circle',
                warning: 'exclamation-circle',
                error: 'exclamation-circle',
                loading: 'loading',
            })[this.type];
        },
        iconClasses() {
            return [
                this.prefix,
                `${this.prefix}-${this.iconType}`,
                { [`${this.prefix}-spin`]: this.spin || this.iconType === 'loading' },
            ];
        },
    },
    mounted() {
        this._clearCloseTimer();
        if (this.duration) {
            this.closeTimer = setTimeout(() => {
                this._close();
            }, this.duration * 1000);
        }
    },
    beforeDestory() {
        this._clearCloseTimer();
    },
    methods: {
        _clearCloseTimer() {
            if (this.closeTimer) {
                clearTimeout(this.closeTimer);
                this.closeTimer = null;
            }
        },
        _close() {
            this._clearCloseTimer();
            this.onClose();
            this.$emit('close', this.selfKey);
        },
    },
    components: {
    },
};
</script>
