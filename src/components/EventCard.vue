<template>
	<SectionCard>
		<template #title >{{title}}</template>
		<template #default>{{when}}</template>
		<template #description>{{description}}
        <div class="flex justify-end mt-2">
            <!-- <button class="text-sm font-medium border border-gray-200 rounded-full px-3 py-1 hover:bg-blue-300" @click="$emit('register')">Register</button> -->
             <Button @click="$emit('register')">Register</Button>

        </div></template>
	</SectionCard>
</template>

<script setup>
import Button from '@/components/Button.vue';
import SectionCard from '@/components/SectionCard.vue';	


defineProps({
	title: String,
	when: String,
	description: String
});

defineEmits(['register','teste']);
</script>

<style scoped></style>
