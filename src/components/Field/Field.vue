<template>
  <div class="field">
    <label :for="id">{{ label }}</label>
    <slot></slot>
    <ErrorMessage/>
  </div>
</template>

<script>

import ErrorMessage from '@/components/Field/ErrorMessage';

export default {
  components: { ErrorMessage },
  props: {
    id: String,
    label: String
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
</style>
