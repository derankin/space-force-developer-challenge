<script setup lang="ts">
import TicketDetails from '@/components/TicketDetails.vue'

import {  ref } from 'vue'

const ticketDetailsToggler = ref(false)

function toggleTicketDetails() {
  ticketDetailsToggler.value = !ticketDetailsToggler.value
}
</script>
<template>
  <div v-if="!ticketDetailsToggler">
    <h1>Tickets</h1>
    <button class="btn btn-primary" @click="toggleTicketDetails">+ Add</button>
  </div>
  <TicketDetails v-if="ticketDetailsToggler" @close="toggleTicketDetails" />
</template>



<style scoped>

</style>
