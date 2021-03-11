<template>
  <span class="error">Введено не корректное значение</span>
</template>
<script>
export default {
  name: 'ErrorMessage'
};
</script>

<style scoped>
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

input.has-error + .error {
  opacity: 1;
  visibility: visible;
}
</style>
