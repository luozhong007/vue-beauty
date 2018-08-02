<template>
    <div class="ant-tooltip-wrapper" style="display:inline-block" @mouseenter="handleShowPopper" @mouseleave="handleClosePopper">
        <div style="display:inline-block" ref="reference">
            <slot></slot>
        </div>
        <transition name="fade">
            <div :class="`ant-tooltip ant-tooltip-placement-${placement}`" :style="tooltipStyle" ref="popper" v-show="!disabled && (visible || always)">
                <div class="ant-tooltip-content">
                    <div class="ant-tooltip-arrow"></div>
                    <div class="ant-tooltip-inner"><slot name="content">{{ content }}</slot></div>
                </div>
            </div>
        </transition>
    </div>
</template>
<script>
    import Popper from '../../mixins/popper';

    const placements = ['top', 'topLeft', 'topRight', 'bottom', 'bottomLeft', 'bottomRight', 'left', 'leftTop', 'leftBottom', 'right', 'rightTop', 'rightBottom'];

    export default {
        name: 'Tooltip',
        mixins: [Popper],
        props: {
            placement: {
                validator(value) {
                    return placements.includes(value);
                },
                default: 'top',
            },
            content: {
                type: [String, Number],
                default: '',
            },
            delay: {
                type: Number,
                default: 0,
            },
            disabled: {
                type: Boolean,
                default: false,
            },
            controlled: { // under this prop,Tooltip will not close when mouseleave
                type: Boolean,
                default: false,
            },
            initVisible: {
                type: Boolean,
                default: false,
            },
            always: {
                type: Boolean,
                default: false,
            },
        },
        data() {
            return {
                prefixCls: 'ant-tooltip',
            };
        },
        computed: {
            tooltipStyle() {
                return {
                    marginLeft: `-${this.getTextWidth(this.content) / 2 + 2}px`,
                };
            },
        },
        methods: {
            handleShowPopper() {
                this.timeout = setTimeout(() => {
                    this.visible = true;
                }, this.delay);
            },
            handleClosePopper() {
                clearTimeout(this.timeout);
                if (!this.controlled) {
                    this.visible = false;
                }
            },
            // 根据文字在html中渲染结果获取宽度
            getTextWidth(text) {
                try {
                    let ele = document.getElementById('sbWidth');
                    if (!ele) {
                        const span = document.createElement('span');
                        span.id = 'sbWidth';
                        span.style.cssText = 'opacity: 0; position: fixed; z-index: -1000; font-size:12px;padding: 0 10px; pointer-events: none; left: 0; top: 0;';
                        document.body.appendChild(span);
                        ele = span;
                    }
                    ele.innerText = text;
                    return ele.offsetWidth;
                } catch (e) {
                    return 0;
                }
            },
        },
        mounted() {
            this.visible = this.initVisible;
        },
    };
</script>

