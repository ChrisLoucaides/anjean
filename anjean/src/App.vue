<template>
  <div>
    <h1>TfL DLR Arrivals</h1>
    <div id="arrival-data">
      <DlrService :filteredData="filteredData" :key="updateKey" />
    </div>
  </div>
</template>

<script>
import DlrService from './components/DlrService.vue';

export default {
  components: {
    DlrService
  },
  data() {
    return {
      jsonData: null,
      filteredData: [],
      updateKey: 0
    };
  },
  created() {
    this.fetchData();
    this.startDataPolling();
  },
  methods: {
    fetchData() {
      // Fetch data from the TfL API
      fetch('https://api.tfl.gov.uk/line/dlr/arrivals')
          .then(response => response.json())
          .then(data => {
            this.jsonData = data;
            this.filteredData = data.filter(entry => entry.stationName === 'Stratford International DLR Station');
            this.updateKey++; // Update the key to trigger the animation
          })
          .catch(error => {
            console.error('Error fetching data:', error);
          });
    },
    startDataPolling() {
      setInterval(this.fetchData, 60000);
    }
  }
};
</script>