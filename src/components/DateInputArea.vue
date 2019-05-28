<template>
  <div class="container">
    <textarea
      class="textarea is-primary"
      placeholder="Input dates, separated by lines, commas or semicolons"
      v-model="rawDates"
      @input="$emit('input',parsedDates)"
    ></textarea>
  </div>
</template>

<script>
export default {
  name: "DateInputArea",
  data: () => ({ rawDates: "" }),
  computed: {
    parsedDates() {
      return this.rawDates.split(/,|\n|;/g).filter(d => d.trim().length);
    }
  }
};
</script>

<style scoped>
.container {
  margin: 1rem;
  display: flex;
  justify-content: center;
  max-width: 100%;
}
</style>
