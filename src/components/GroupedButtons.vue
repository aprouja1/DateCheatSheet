<template>
  <div>
    <div class="group-container">
      <label class="label">{{label}}</label>
      <div class="field has-addons">
        <p class="control" v-for="option in options" :key="option.value">
          <button
            class="button is-primary is-small"
            :class="[value===option.value?'is-selected':'is-outlined']"
            @click="$emit('input',option.value)"
          >
            <span class="icon is-small">
              <i :class="option.iconClass"></i>
            </span>
            <span>{{option.label}}</span>
          </button>
        </p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "GroupedButtons",
  props: {
    options: { type: Array, required: true },
    label: { type: String, required: true },
    value: { type: String, required: true }
  }
};
</script>

<style scoped>
.group-container {
  margin: 0 !important;
  flex-grow: 0 !important;
}

.button span {
  display: flex;
  align-items: center;
}
