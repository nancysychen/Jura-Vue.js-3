<template>
	<main class="container mx-5 my-8 space-y-8">
	<h1 class="text-4xl font-medium">Event Booking App</h1>
	<h2 class="text-2xl font-medium">All Events</h2>
	<section class="grid grid-cols-2 gap-3">
		<EventCard v-for="i in 8" :key="i" title="Vue Conference 2024" when="2024-05-01" description="Conference about Vue and JavaScript" @register="console.log('testando emitt de registro')"/>
	</section>
	<h2 class="text-2xl font-medium">Your Bookings</h2>
	<section class="grid grid-cols gap-2">
		<BookingItem v-for="b in 3" :key="b" @cancelar="console.log(' testando emit de cancelar');"/>
	</section>
		</main>
</template>

<script setup>
import EventCard from '@/components/EventCard.vue';
import BookingItem from '@/components/BookingItem.vue';
</script>

