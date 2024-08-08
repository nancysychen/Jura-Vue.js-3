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
				@register="handleRegistration(event)"
			/>
		</section>
		<section v-else class="grid grid-cols-2 gap-3">
			<LoadingCard v-for="i in 4" :key="i" />
		</section>

		<h2 class="text-2xl font-medium ml-5">Your Bookings</h2>
		<section class="grid gap-2" v-if="!bookingsLoading">
			<BookingItem
				v-for="booking in bookings"
				:key="booking.id"
				:title="booking.eventTitle"
				@cancelar="console.log(' testando emit de cancelar')"
			/>
		</section>
		<section v-else class="grid gap-2">
			<LoadingBookings v-for="i in 4" :key="i"/>
		</section>
	</main>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import EventCard from '@/components/EventCard.vue';
import BookingItem from '@/components/BookingItem.vue';
import LoadingCard from './components/LoadingCard.vue';
import LoadingBookings from './components/LoadingBookings.vue';

const events = ref([]);
const bookings = ref([]);
const eventsLoading = ref(false);
const bookingsLoading = ref(false);

const fetchEvents = async () => {
	eventsLoading.value = true;
	try {
		const response = await fetch('http://localhost:3001/events');
		events.value = await response.json();
	} finally {
		eventsLoading.value = false;
	}
};

const fetchBookings = async () => {
	bookingsLoading.value = true;	
	try {
		const response = await fetch('http://localhost:3001/bookings');
		bookings.value = await response.json();
		
	} finally {
		bookingsLoading.value = false;
		
	}
};
onMounted(() => {
	fetchEvents();
	fetchBookings();
});



const handleRegistration = async (event) => {
	const newBooking = {
		id: Date.now().toString(),
		userId: 1,
		eventId: event.id,
		eventTitle: event.title,
	};
	await fetch('http://localhost:3001/bookings', {
		method: 'POST',
		headers: { 'Content-Type': 'application/json' },
		body: JSON.stringify({
			...newBooking,
			status: 'confirmed',
		}),
	});
};
</script>
