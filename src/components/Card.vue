<template>
  <div class="container">
    <div class="card">
      <header class="card-header has-background-primary">
        <p class="card-header-title has-text-white">{{title}}</p>
        <span class="card-header-icon">
          <span class="icon">
            <i :class="timeRegex.test(title)?'icon-clock':'icon-calendar'"></i>
          </span>
        </span>
      </header>
      <div class="card-content" @scroll="$emit('scroll',$event.target.scrollTop)">
        <div class="content">
          <table class="table is-striped is-fullwidth">
            <thead>
              <tr>
                <th class="has-text-primary">{{groupMode==='method'?"Date":"Method"}}</th>
                <th class="has-text-primary">Result</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="({label, result},i) in results" :key="label+i">
                <td>{{label}}</td>
                <td>{{result()}}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Card",
  props: {
    title: {
      type: String,
      required: true
    },
    dates: {
      type: Array,
      required: true
    },
    function: {
      type: [Array, String],
      required: true
    },
    groupMode: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      timeRegex: new RegExp("time|second|hour|minute", "gi")
    };
  },
  computed: {
    results() {
      if (this.groupMode === "method") {
        return this.dates.map(d => ({
          label: d,
          result: () => {
            try {
              return Date.prototype[this.function].apply(new Date(d));
            } catch (error) {
              return "Invalid Date";
            }
          }
        }));
      } else {
        return this.function.map(f => ({
          label: f,
          result: () => {
            try {
              return Date.prototype[f].apply(new Date(this.title));
            } catch (error) {
              return "Invalid Date";
            }
          }
        }));
      }
    }
  }
};
</script>

<style scoped>
i {
  color: white;
  font-weight: bolder;
  font-size: 1.5rem;
  cursor: default;
}
.icon,
.card-header-icon {
  cursor: default;
}
table {
  table-layout: fixed;
  border-spacing: 10px;
}
.container {
  display: contents;
}
td {
  padding: 0.5rem 0 !important;
}

.card {
  max-height: 50vh;
}
header {
  height: 7vh;
}

.card-content {
  overflow-y: auto;
  max-height: 43vh;
}
</style>
