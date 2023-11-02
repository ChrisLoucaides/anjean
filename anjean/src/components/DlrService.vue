<!--suppress JSUnresolvedReference -->
<template>
  <div class="">
    <br>
    <h1>DLR Services: </h1>
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
              <h2>Expected Departure Time: {{ getBSTTime(arrival.expectedArrival) }}</h2>

              <!--TODO: Fix me-->
              <h2>Leaving in:
                <span class="minutes-to-next-train">
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
      date.setMinutes(date.getMinutes() + 2) //TODO: Refactor
      date.setHours(date.getHours());
      return date.toLocaleTimeString('en-GB', {hour: '2-digit', minute: '2-digit'});
    },

    stripDlrNamespace(stationName) {
      return stationName.replace("DLR Station", "")
    },

    timeToNextTrain(expectedArrival) {

      let expectedArrivalDate = new Date(expectedArrival);
      expectedArrivalDate.setMinutes(expectedArrivalDate.getMinutes() + 2) //TODO: Refactor
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

.minutes-to-next-train {
  color: #19b6b1;
}
</style>
