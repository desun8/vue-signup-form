<!--поле “Имя” не может содержать цифры-->
<!--и символы кроме пробела и дефиса-->
<!--поле “email” можно отправить только-->
<!--email.-->
<!--поле “номер телефона” можно ввести только 11 цифр,-->
<!--круглые скобки, дефис и знак плюс.-->

<template>
  <input :id="id" v-model.trim="value" @blur="handleBlur" @input="handleInput" :class="{'has-error': isError}"
         type="text" name="name" autocomplete="name" placeholder="Введите Ваше имя">
</template>

<script>
export default {
  name: 'Name',
  props: {
    id: {
      type: String,
      required: true
    },
    setHasError: {
      type: Function,
      required: true
    }
  },
  data() {
    return {
      value: '',
      isError: false
    };
  },

  methods: {
    validation() {
      const regexp = /^[a-zа-я]+(([ -][a-zа-я])?[a-zа-я]*)*$/i;
      const hasError = this.value.match(regexp) === null;
      return this.value !== '' ? hasError : false;
    },

    handleInput() {
      if (this.isError) {
        this.isError = this.validation();
        this.setHasError(this.isError);
      }
    },

    handleBlur() {
      this.isError = this.validation();
      this.setHasError(this.isError);
    },
  }
};
</script>

<style scoped>

</style>
