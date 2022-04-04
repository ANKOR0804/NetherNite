<template>
<div class="module-more" v-if="show" @click.stop="hideMore">
    <div @click.stop class="module-more__info">
        <slot></slot>
    </div>
</div>
</template>

<script>
export default {
    name: 'extended-information',
    props: {
        show: {
            type: Boolean,
            default: false
        }
    },
    methods: {
        hideMore() {
            this.$emit('update:show', false);
        },
        findModule() {
            
        }
    }
}
</script>

<style lang="scss" scoped>

    .module-more {
        top: 0;
        right: 0;
        bottom: 0;
        left: 0;
        background: rgba(0,0,0,0.5);
        position: fixed;
        display: flex;

        &__info {
            margin: auto;
            background: white;
            border-radius: 12px;
            min-height: 50px;
            min-width: 300px;
        }
    }
</style>