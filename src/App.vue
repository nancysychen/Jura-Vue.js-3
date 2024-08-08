<template>
	<main class="container mx-auto my-8 space-y-5">
		<h1 class="text-4xl font-medium ml-5">Event Booking App</h1>
		<h2 class="text-2xl font-medium ml-5">All Events</h2>
		<section class="grid grid-cols-2 gap-3" v-if="!eventsLoading">
			<EventCard
				v-for="event in events"
				:key="event.id"
				:title="event.title"
				:when="event.date"
				:description="event.description"
				@register="registerEvent"
			/>
		</section>
		<section v-else class="grid grid-cols-2 gap-3">
			<LoadingCard v-for="i in 4" :key="i"/>
		</section>

		<h2 class="text-2xl font-medium ml-5">Your Bookings</h2>
		<section class="grid grid-cols gap-2">
			<BookingItem
				v-for="b in 3"
				:key="b"
				@cancelar="console.log(' testando emit de cancelar')"
			/>
		</section>
	</main>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import EventCard from '@/components/EventCard.vue';
import BookingItem from '@/components/BookingItem.vue';
import LoadingCard from './components/LoadingCard.vue';

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
onMounted(() => {
	fetchEvents();
});

const registerEvent = () => {
	console.log('evento registrado');
};
</script>
