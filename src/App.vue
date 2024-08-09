<template>
	<main class="container mx-auto my-8 space-y-5">
		<h1 class="text-4xl font-medium ml-5">Event Booking App</h1>
		<h2 class="text-2xl font-medium ml-5">All Events</h2>
		<EventList @register="handleRegistration($event)"/>

		<h2 class="text-2xl font-medium ml-5">Your Bookings</h2>
		<section class="grid gap-2" v-if="!bookingsLoading">
			<BookingItem
				v-for="booking in bookings"
				:key="booking.id"
				:title="booking.eventTitle"
				:status="booking.status"
				@cancelar="cancelBooking(booking.id)"
			/>
		</section>
		<section v-else class="grid gap-2">
			<LoadingBookings v-for="i in 4" :key="i" />
		</section>
	</main>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import BookingItem from '@/components/BookingItem.vue';
import LoadingBookings from './components/LoadingBookings.vue';
import EventList from './components/EventList.vue';

const bookings = ref([]);

const bookingsLoading = ref(false);
const findBookingById = (id) => {  
	return bookings.value.findIndex((b) => b.id === id);
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
	fetchBookings();
});

const handleRegistration = async (event) => {
	if (
		bookings.value.some(
			(booking) => booking.eventId === event.id && booking.userId === 1
		)
	) {
		alert('You are already registered.');
		return;
	}
	const newBooking = {
		id: Date.now().toString(),
		userId: 1,
		eventId: event.id,
		eventTitle: event.title,
		status: 'pending',
	};
	bookings.value.push(newBooking);

	try {
		const response = await fetch('http://localhost:3001/bookings', {
			method: 'POST',
			headers: { 'Content-Type': 'application/json' },
			body: JSON.stringify({
				...newBooking,
				status: 'confirmed',
			}),
		});
		if (response.ok) {
			const index = findBookingById(newBooking.id);
			bookings.value[index] = await response.json();
		} else {
			throw new Error('Failed to confirm booking');
		}
	} catch (error) {
		console.error(`Failed to register for event: `, e);
		bookings.value.filter((b) => b.id !== newBooking.id);
	}
};
const cancelBooking = async (bookingId) => {
	const index = findBookingById(bookingId);
	const originalBooking = bookings.value[index];
	bookings.value.splice(index, 1);

	try {
		const response = await fetch(`http://localhost:3001/bookings/${bookingId}`, {
			method: 'DELETE'
		});
		if (!response.ok) {
			throw new Error('Booking cannot be canceled');
		}
	} catch (error) {
		console.error(`Failed to cancel booking: `, error);
		bookings.value.splice(index, 0, originalBooking);
	}
};
</script>
