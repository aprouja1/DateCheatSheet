<template>
  <div class="field">
    <label :for="name">{{labels[0]}}</label>
    <input
      :id="name"
      type="checkbox"
      :name="name"
      class="switch is-rounded is-outlined"
      @input="$emit('input',$event.target.checked ? onValue:offValue)"
      :checked="initialCheck"
    >
    <label :for="name">{{labels[1]}}</label>
  </div>
</template>

<script>
export default {
  name: "Toggle",
  props: {
    value: {
      type: [String, Boolean],
      required: true
    },
    onValue: {
      type: [String, Boolean],
      required: false,
      default: true
    },
    offValue: {
      type: [String, Boolean],
      required: false,
      default: false
    },
    name: { type: String, required: true },
    labels: {
      type: Array,
      required: false,
      default: () => ["Off", "On"]
    }
  },
  computed: {
    initialCheck() {
      return this.value === this.onValue  ? true : false;
    }
  }
};
</script>

<style scoped>
label::after,
label::before {
  outline: 0 !important;
}
label:first-of-type {
  margin-right: 5%;
}
</style>
