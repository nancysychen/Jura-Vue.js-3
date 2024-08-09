<template>
    <section class="grid grid-cols-2 gap-3" v-if="!eventsLoading">
			<EventCard
				v-for="event in events"
				:key="event.id"
				:title="event.title"
				:when="event.date"
				:description="event.description"
				@register="$emit('register',event)"
			/>
		</section>
		<section v-else class="grid grid-cols-2 gap-3">
			<LoadingCard v-for="i in 4" :key="i" />
		</section>
</template>

<script setup>
defineEmits(['register'])
import { ref, onMounted } from 'vue';
import EventCard from './EventCard.vue';
import LoadingCard from './LoadingCard.vue';

const events = ref([]);
const eventsLoading = ref(false);

const fetchEvents = async () => {
	eventsLoading.value = true;
	try {
		const response = await fetch('http://localhost:3001/events');
		events.value = await response.json();
	} finally {
		eventsLoading.value = false;
	}
};
onMounted (()=>{
	fetchEvents();
}) ;

</script>

<style scoped>

</style>