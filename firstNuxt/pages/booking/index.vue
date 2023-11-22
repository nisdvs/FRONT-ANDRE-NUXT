
<script setup>
    const {data: bookings} =  await useFetch('http://localhost:8000/booking' ,{
        key: 'loadBooking'
    });

    const bookingDelete = async (idToDelete) => {
        //console.log("id to delete:", idToDelete)
        const {} = await useFetch(`http://localhost:8000/booking/$(idToDelete)`, {
            method: 'DELETE',
            onResponse(){
                alert("Booking Succesfuly deleted");
                refreshNuxtData('loadBooking')

            },
            onResponseError(){
                alert("Was not possible to delete this booking");
            }
        });
    }

</script>
<template>
    <div>
        <h1>
            My booking List:
        </h1>
        <div v-for="booking in bookings.results" :key="booking.id">
            <span>Booking ID: {{ booking.id }} , </span>
            <span>Trip: {{ booking.tripFK }} , </span>
            <span>Start date: {{ booking.startDate }} , </span>
            <span> End date: {{ booking.endDate}} , </span>
            <span> Customer: {{ booking.customUserFK}} </span> <br>
            <button @click="bookingDelete(booking.id)">Delete</button>
            <br>
        </div>
    </div>
</template>