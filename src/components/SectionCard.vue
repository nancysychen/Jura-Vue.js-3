<template>
	<SingleCard>
        <div class="border border-gray-200 bg-white rounded-md">
            <h3 v-if="$slots.title" class="p-4 font-medium text-xl border-b border-gray-200">
                <slot name="title"></slot>
            </h3>
            <div v-if="$slots.default" class="p-4 border-b border-gray-200"><slot></slot></div>
            <div v-if="$slots.description" class="p-4"><slot name="description"></slot></div>
        </div>
  
    </SingleCard>
</template>

<script setup>
    import SingleCard from '@/components/SingleCard.vue';

    
</script>

<style scoped></style>
