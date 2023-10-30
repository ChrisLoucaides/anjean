<template>
  <div id="arrival-data" class="container d-flex align-items-center justify-content-center">
    <DlrService :filteredData="filteredData" :key="updateKey"/>
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
      fetch('https://api.tfl.gov.uk/line/dlr/arrivals')
          .then(response => response.json())
          .then(data => {
            this.jsonData = data;
            this.filteredData = data.filter(entry => entry.stationName === 'Stratford International DLR Station');
            this.updateKey++;
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
<style>
body {
  background-image: url("./assets/DLR_roundel.svg");
  background-repeat: no-repeat;
  background-position: right;
  background-size: 70%;
  opacity: 0.95;
}
</style>