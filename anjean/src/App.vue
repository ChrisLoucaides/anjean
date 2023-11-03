<template>
  <div id="arrival-data" class="container d-flex align-items-center justify-content-center">
    <DlrService :filteredData="filteredData" :key="updateKey"/>
  </div>
</template>

<script>
import DlrService from './components/DlrService.vue';
import TimeComponent from "@/components/TimeComponent.vue";


export default {
  components: {
    DlrService,
    TimeComponent
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
.dlr-logo-container {

}

/*TODO: Change logo in PS, and use as background*/
.dlr-logo {
  width: 80%;
  z-index: -1;
  opacity: 0.9;
  margin-bottom: 1vh;
}

body {
  background-image: url("./assets/DLR_roundel.svg");
  background-repeat: no-repeat;
  background-attachment: fixed;
  background-position: right;
  background-size: 60%;

}

</style>