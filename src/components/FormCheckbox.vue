<script>
export default {
  name: "form-checkbox",
  props: {
    modelValue: Boolean,
    name: {
      type: String,
      required: true,
    },
    label: String,
  },
  emits: ["update:modelValue"],
  computed: {
    validationMessage() {
      return this.errors.concat(this.warnings)[0];
    },
    inputValue: {
      get: (props) => props.modelValue,
      set(newVal) {
        this.$emit("update:modelValue", newVal, this.$props.name);
      },
    },
  },
};
</script>

<template>
  <label :for="name" class="form-checkbox">
    <input type="checkbox" :name="name" v-model="inputValue" />
    <span>{{ label }}</span>
  </label>
</template>

<style scoped>
.form-checkbox {
  font-size: 0.8em;
  padding: 1em 0;
}

.form-checkbox {
  display: flex;
  align-items: center;
}

.form-checkbox input {
  margin-right: 0.5em;
}

.form-checkbox.error {
  color: var(--color-error);
}

.form-checkbox.success {
  color: var(--color-success);
}
</style>
