<template>
  <div class="field">
    <label for class="label">Search Methods</label>
    <div class="control has-icons-left">
      <input
        class="input is-primary is-rounded"
        :value="value"
        type="text"
        placeholder="Search"
        @input="$emit('input',$event.target.value)"
      >
      <span class="icon is-small is-left">
        <i class="icon-magnifier"></i>
      </span>
    </div>
  </div>
</template>

<script>
export default {
  name: "SearchBar",
  props: ["value"]
};
</script>

<style scoped>
.field {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
