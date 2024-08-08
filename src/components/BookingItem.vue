<template>
	<SectionCard>
	
			<div class="flex justify-between">
				<div>Booking to Vue Conference</div>
				<Button variant="danger" @click="$emit('cancelar')">Cancel</Button>
			</div>

	</SectionCard>
</template>

<script setup>
import Button from '@/components/Button.vue';
import SectionCard from './SectionCard.vue';

defineEmits(['cancelar']);
</script>

<style scoped></style>
