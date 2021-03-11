<template>
  <button class="btn" type="submit" :disabled="isDisabled">Зарегистрироваться</button>
</template>

<script>
export default {
  name: 'SubmitBtn',
  props: {
    isDisabled: Boolean
  }
};
</script>

<style scoped>
button {
  padding: 18px;
  color: #fff;
  background-color: var(--c-blue);
  font-weight: 500;
  border: 2px solid var(--c-blue);
  border-radius: 6px;
  box-shadow: 0 2px 4px rgba(44, 39, 56, 0.08), 0 4px 8px rgba(44, 39, 56, 0.08);
  transition: 0.3s ease;
  transition-property: box-shadow, background-color, color, border-color;
}

button:focus:not(:focus-visible) {
  outline: none;
}

button:hover {
  box-shadow: 0 12px 24px rgba(44, 39, 56, 0.08), 0 24px 48px rgba(44, 39, 56, 0.16);
}

button:active {
  border-color: rgba(44, 39, 56, 0.86);
}

button:disabled {
  color: rgba(44, 39, 56, 0.5);
  background: #dbe2ea;
  border-color: #dbe2ea;
  box-shadow: 0 4px 8px rgba(44, 39, 56, 0.08);
}

</style>
