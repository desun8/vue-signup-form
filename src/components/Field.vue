<template>
  <div class="field">
    <label :for="id">{{ name }}</label>
    <slot></slot>
    <span class="error">Введено не корректное значение</span>
  </div>
</template>

<script>
export default {
  name: 'Field',
  props: {
    id: {
      type: String,
      required: true
    },
    name: String
  }
};
</script>

<style scoped>
.field {
  position: relative;
  display: grid;
  gap: 6px;
}

label {
  color: var(--c-text);
  font-weight: 500;
}

label::first-letter {
  text-transform: capitalize;
}

.field >>> input {
  width: 100%;
  padding: 16px;
  border: 1px solid #dbe2ea;
  border-radius: 6px;
  box-shadow: 0 4px 8px rgba(44, 39, 56, 0.04);
}

.field >>> input::placeholder {
  color: #7c9cbf;
}

.field >>> input:focus {
  outline: 0;
  border-color: var(--c-blue);
  box-shadow: 0 4px 8px rgba(44, 39, 56, 0.04), inset 1px 0 0 var(--c-blue), inset 0 1px 0 var(--c-blue), inset -1px 0 0 var(--c-blue), inset 0 -1px 0 var(--c-blue);
}

.field >>> input:-webkit-autofill,
.field >>> input:-webkit-autofill:hover,
.field >>> input:-webkit-autofill:focus {
  background-color: transparent !important;
  color: inherit !important;
  -webkit-box-shadow: 0 0 0 50px white inset;
}

.error {
  position: absolute;
  top: calc(100% + 8px);
  left: 0;
  right: 0;

  color: #ff7171;
  font-size: 14px;

  opacity: 0;
  visibility: hidden;
  transition-property: opacity, visibility;
  transition-duration: 0.2s;
}

.field >>> input.has-error + .error {
  opacity: 1;
  visibility: visible;
}
</style>
