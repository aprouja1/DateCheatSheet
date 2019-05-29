<template>
  <div class="field">
    <label class="label is-centered">Row Height (vh)</label>
    <div class="control">
      <input
        class="input is-primary"
        type="number"
        placeholder="Height"
        @input="$emit('input',$event.target.value)"
        :value="value"
      >
    </div>
    <p class="help is-primary">0 is auto</p>
  </div>
</template>

<script>
export default {
  name: "HeightInput",
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
