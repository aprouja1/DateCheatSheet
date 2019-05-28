<template>
  <div id="app">
    <div class="settings-row">
      <div class="toggle-container">
        <label for class="label">Group By</label>
        <toggle
          v-model="groupMode"
          onValue="method"
          offValue="dates"
          name="groupBy"
          :labels="['Date','Method']"
        />
      </div>
      <div class="toggle-container" v-show="groupMode==='dates'">
        <label for class="label">Scroll All</label>
        <toggle v-model="shouldScrollAll" name="scroll"/>
      </div>
      <grouped-buttons :options="filterOptions" label="Filter to" v-model="filterTo"/>
      <search-bar v-model="methodSearch"/>
    </div>
    <date-input-area v-model="dates"/>
    <div class="card-grid">
      <keep-alive v-for="(property,i) in groupBy" :key="property+i">
        <card
          :title="property"
          :dates="dates"
          :function="groupMode==='method' ? property : dateFunctionsWithoutSetters"
          :groupMode="groupMode"
          @scroll="scrollAll"
        />
      </keep-alive>
    </div>
  </div>
</template>

<script>
import Card from "./components/Card.vue";
import Toggle from "./components/Toggle.vue";
import SearchBar from "./components/SearchBar.vue";
import DateInputArea from "./components/DateInputArea.vue";
import GroupedButtons from "./components/GroupedButtons.vue";
export default {
  name: "App",
  components: {
    Card,
    Toggle,
    SearchBar,
    DateInputArea,
    GroupedButtons
  },
  data() {
    return {
      timeRegex: new RegExp("time|second|hour|minute", "i"),
      dates: [],
      groupMode: "method",
      methodSearch: "",
      shouldScrollAll: true,
      filterOptions: [
        { iconClass: "icon-globe", label: "All", value: "all" },
        { iconClass: "icon-calendar", label: "Dates", value: "dates" },
        { iconClass: "icon-clock", label: "Times", value: "times" }
      ],
      filterTo: "all"
    };
  },
  computed: {
    dateFunctions: () =>
      Object.getOwnPropertyNames(Date.prototype).filter(
        d => d !== "constructor"
      ),
    dateFunctionsWithoutSetters() {
      const functions = this.dateFunctions
        .filter(d => !d.startsWith("set"))
        .filter(d => d.toLowerCase().includes(this.methodSearch.toLowerCase()));
      if (this.filterTo === "times") {
        return functions.filter(f => this.timeRegex.test(f));
      } else if (this.filterTo === "dates") {
        return functions.filter(f => !this.timeRegex.test(f));
      }
      return functions;
    },
    groupBy() {
      return this.groupMode === "method"
        ? this.dateFunctionsWithoutSetters
        : this.dates;
    }
  },
  methods: {
    scrollAll(h) {
      this.shouldScrollAll &&
        document
          .querySelectorAll(".card-content")
          .forEach(c => (c.scrollTop = h));
    }
  }
};
</script>

<style>
.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(31vw, 1fr));
  grid-gap: 1rem;
  margin: 0 1rem;
  transition: all 1s;
}
.toggle-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 1rem;
}
.settings-row {
  display: flex;
  justify-content: space-around;
}
.group-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
