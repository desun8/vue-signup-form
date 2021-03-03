<template>
  <input :id="id" v-model.trim="value" @blur="handleBlur" @input="handleInput" :class="{'has-error': isError}"
         type="email" name="email" autocomplete="email" placeholder="Введите ваш email">
</template>

<script>
export default {
  name: 'Email',
  props: {
    id: {
      type: String,
      required: true
    },
  },
  data() {
    return {
      value: '',
      isError: false
    };
  },

  methods: {
    validation() {
      const regexp = /(?:[a-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-z0-9!#$%&'*+/=?^_`{|}~-]+)*|"*")@(?:(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?\.)+[a-z0-9](?:[a-z0-9-]*[a-z0-9])?|\[(?:(?:(2(5[0-5]|[0-4][0-9])|1[0-9][0-9]|[1-9]?[0-9]))\.){3}(?:(2(5[0-5]|[0-4][0-9])|1[0-9][0-9]|[1-9]?[0-9])|[a-z0-9-]*[a-z0-9]+)\])/igm;
      const hasError = this.value.match(regexp) === null;
      return this.value !== '' ? hasError : false;
    },

    handleInput() {
      if (this.isError) {
        this.isError = this.validation();
      }
    },

    handleBlur() {
      this.isError = this.validation();
    },
  }
};
</script>

<style scoped>

</style>
