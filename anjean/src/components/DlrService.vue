<template>
  <div>
    <h2>DLR Service Information</h2>
    <transition name="fade">
      <div v-if="sortedData.length > 0">
        <ul>
          <div v-for="arrival in sortedData" :key="arrival.id">
            <p><strong>Station Name:</strong> {{ arrival.stationName }}</p>
            <p><strong>Destination Name:</strong> {{ arrival.destinationName }}</p>
            <p><strong>Expected Departure Time:</strong> {{ getBSTTime(arrival.expectedArrival) }}</p>
            <hr>
            <br>
          </div>
        </ul>
      </div>
      <div v-else>
        <p>No arrivals found for Stratford International DLR Station</p>
      </div>
    </transition>
  </div>
</template>

<script>
export default {
  props: {
    filteredData: Array // Define the prop for filtered data
  },
  computed: {
    sortedData() {
      return this.filteredData.slice().sort((a, b) => {
        const dateA = new Date(a.expectedArrival);
        const dateB = new Date(b.expectedArrival);
        return dateA - dateB;
      });
    }
  },
  methods: {
    getBSTTime(iso8601) {
      const date = new Date(iso8601);
      // Adjust for BST (GMT+1)
      date.setHours(date.getHours());
      // Format the time to HH:mm
      return date.toLocaleTimeString('en-GB', { hour: '2-digit', minute: '2-digit' });
    }
  }
};
</script>

<style>
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s;
}

.fade-enter, .fade-leave-to
{
  opacity: 0;
}
</style>
