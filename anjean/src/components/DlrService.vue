<template>
  <div>
    <h2>DLR Service Information</h2>
    <div v-if="filteredData.length > 0">
      <ul>
        <li v-for="arrival in filteredData" :key="arrival.id">
          <p><strong>Station Name:</strong> {{ arrival.stationName }}</p>
          <p><strong>Destination Name:</strong> {{ arrival.destinationName }}</p>
          <p><strong>Expected Departure Time:</strong> {{ arrival.expectedArrival }}</p>
          <hr>
        </li>
      </ul>
    </div>
    <div v-else>
      <p>No arrivals found for Stratford International DLR Station</p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    filteredData: Array
  }
};
</script>
