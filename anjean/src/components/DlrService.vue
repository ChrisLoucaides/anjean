<!--suppress JSUnresolvedReference -->
<template>
  <div class="">
    <br>
    <div class="heading">
      <h2>Current Time:&nbsp;</h2>
      <TimeComponent></TimeComponent>
    </div>
    <br>
    <transition name="fade">
      <div class="cards" v-if="sortedData.length > 0">
        <div v-for="arrival in sortedData" :key="arrival.id">
          <div class="card text-bg-light mb-3 dlr-card" style="max-width: 100em;">
            <div class="card-header"><h2>Service to: {{ stripDlrNamespace(arrival.destinationName) }} <img
                src="../assets/DLR_roundel.svg" alt="" style="width: 1.2em">
            </h2>
            </div>
            <div class="card-body">
              <h2>Expected Departure Time: <span class="dlr-color">
                <strong>{{ getBSTTime(arrival.expectedArrival) }}</strong>
              </span></h2>

              <h2>Leaving in:
                <span class="dlr-color">
                  <strong>({{ timeToNextTrain(arrival.expectedArrival) }} mins)</strong>
                </span>
              </h2>
              <h5 class="card-title">Departing from: {{ stripDlrNamespace(arrival.stationName) }}</h5>
            </div>
          </div>
          <br>
        </div>
      </div>
      <div v-else>
        <p>No arrivals found for Stratford International DLR Station</p>
      </div>
    </transition>
  </div>
</template>

<script>
import TimeComponent from "@/components/TimeComponent.vue";

export default {
  components: {
    TimeComponent
  },

  props: {
    filteredData: Array
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
      let date = new Date(iso8601);
      date.setMinutes(date.getMinutes())
      date.setHours(date.getHours());
      return date.toLocaleTimeString('en-GB', {hour: '2-digit', minute: '2-digit'});
    },

    stripDlrNamespace(stationName) {
      return stationName.replace("DLR Station", "")
    },

    timeToNextTrain(expectedArrival) {
      let expectedArrivalDate = new Date(expectedArrival);
      expectedArrivalDate.setMinutes(expectedArrivalDate.getMinutes())
      return Math.round(this.convertToMinutes(expectedArrivalDate - new Date()));
    },

    convertToMinutes(timeToNextTrainInDateFormat) {
      return timeToNextTrainInDateFormat / (1000 * 60);
    }
  }
};
</script>

<style>
/*noinspection ALL*/
.fade-enter-active, .fade-leave-active {
  transition: opacity 2s;
}

/*noinspection CssUnusedSymbol*/
.fade-enter, .fade-leave-to {
  opacity: 0;
}

.cards {
  z-index: 1;
}

.dlr-card {
  opacity: 0.90;
}

.dlr-color {
  color: #19b6b1;
}

.heading {
  display: flex;
  flex-direction: row;
}
</style>
