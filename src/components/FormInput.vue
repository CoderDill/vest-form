<script>
export default {
  name: "form-input",
  props: {
    pending: {
      type: Boolean,
      default: false,
    },
    type: {
      type: String,
      default: "text",
    },
    modelValue: String,
    name: {
      type: String,
      required: true,
    },
    errors: {
      type: Array,
      default: () => [],
    },
    warnings: {
      type: Array,
      default: () => [],
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
  <div class="form-input" :class="{ pending }">
    <label :for="name">
      <strong>{{ label || name }}</strong>
      <span class="validation-message" v-if="validationMessage">{{
        validationMessage
      }}</span>
    </label>
    <input :name="name" v-model="inputValue" />
  </div>
</template>

<style scoped>
label {
  display: block;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

.form-input {
  padding: 1em 0;
  position: relative;
}

.form-input label {
  display: block;
}

.form-input strong {
  text-transform: uppercase;
}

.form-input .validation-message {
  font-weight: 400;
  float: right;
}

.form-input input {
  border-radius: 5px;
  box-sizing: border-box;
  border: 1px solid var(--color-content-inactive);
  background: #fbfbfb;
  width: 100%;
  font-size: 0.9em;
  padding: 1em 1em;
  outline: none;
  transition: 0.2s;
  color: var(--color-content-active);
}

.form-input.pending::after {
  content: "";
  width: 25px;
  height: 25px;
  border-radius: 50%;
  display: block;
  position: absolute;
  transform-origin: 50% 50%;
  right: 1em;
  bottom: 2em;
  animation: spin 1s infinite linear;
  background: conic-gradient(
    #ef476f 0deg,
    #ffd166 60deg,
    #ffd166 90deg,
    #06d6a0 150deg,
    #06d6a0 190deg,
    #118ab2 230deg,
    #118ab2 280deg,
    #ef476f 320deg,
    #ef476f 360deg
  );
}

.form-input input:focus {
  border: 1px solid var(--color-content-active);
}

.form-input.success .validation-message {
  color: var(--color-success);
}

.form-input.warning .validation-message {
  color: var(--color-warning);
}

.form-input.error .validation-message {
  color: var(--color-error);
}

.form-input.success input {
  border: 1px solid var(--color-success);
  color: var(--color-success);
}

.form-input.warning input {
  border: 1px solid var(--color-warning);
  color: var(--color-warning);
}

.form-input.error input {
  border: 1px solid var(--color-error);
  color: var(--color-error);
}
</style>
