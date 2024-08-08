<template>
    <button :class="['text-sm font-medium rounded-full px-3 py-1', variants[variant] ?? variants.default]" @click="$emit('click')">
        <slot>OK</slot>
    </button>

</template>

<script setup>
const variants = {
    default: 'bg-cyan-600 border border-gray-200 hover:bg-blue-300',
    danger:'bg-red-200 border border-red-200 enabled:hover:bg-red-500' 
};

defineProps({
    variant: {
        type: String,
        default: 'default'
    }
});
defineEmits(['click'])

</script>

<style scoped>

</style>