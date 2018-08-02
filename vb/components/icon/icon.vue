<template>
    <i :class="iconClasses">
        <slot></slot>
    </i>
</template>

<script>
    export default {
        name: 'Icon',
        data: () => ({
            prefix: 'anticon',
        }),
        props: {
            type: {
                type: String,
                require: true,
            },
            aaa: {
                type: String,
                require: true,
            },
            spin: {
                type: Boolean,
                default: false,
            },
        },
        watch: {
            aaa(r) {
                console.log(r);
            },
        },
        computed: {
            iconClasses() {
                console.log(this.type);
                return [
                    this.prefix,
                    `${this.prefix}-${this.type}`,
                    { [`${this.prefix}-spin`]: this.spin || this.type === 'loading' },
                ];
            },
        },
    };
</script>
