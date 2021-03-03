<template>
  <input :id="id" v-model.trim="value" @blur="handleBlur" @input="handleInput" :class="{'has-error': isError}"
         type="tel" name="tel" autocomplete="tel" placeholder="Введите номер телефона">
</template>

<script>
export default {
  name: 'Phone',
  props: {
    id: {
      type: String,
      required: true
    },
  },
  data() {
    return {
      value: '',
      prevValue: '',
      isError: false
    };
  },

  methods: {
    validation() {
      let isMatch = false;
      let isFill = false;

      const regexp = /^\+?\d[(]?[0-9]{3}[)]?[-0-9]*\d$/g;
      isMatch = this.value.match(regexp) !== null;

      // Проверка на наличие парных "(" и ")".
      if (isMatch) {
        const openRoundBrackets = this.value.search(/\(/) !== -1;
        const closeRoundBrackets = this.value.search(/\)/) !== -1;
        const hasBrackets = openRoundBrackets && closeRoundBrackets;

        if (!hasBrackets) {
          const onlyOpenBrackets = openRoundBrackets && !closeRoundBrackets;
          const onlyCloseBrackets = !openRoundBrackets && closeRoundBrackets;

          // Если скобка не парная (например забыли), то добавляем.
          if (onlyOpenBrackets) {
            const pos = this.value.search(/\(/) + 4;
            this.value = this.value.slice(0, pos) + ')' + this.value.slice(pos);
          } else if (onlyCloseBrackets) {
            const pos = this.value.search(/\)/) - 3;
            this.value = this.value.slice(0, pos) + '(' + this.value.slice(pos);
          }
        }
      }

      if (isMatch) {
        const numbers = this.value.replace(/[.\-+(d)]/g, '');
        isFill = numbers.length === 11;
      }

      const hasError = !(isMatch && isFill);
      return this.value !== '' ? hasError : false;
    },

    handleInput() {
      const regex = /^\+?\d?[(]?\d{0,3}[)]?(\d-?)?(\d-?)?(\d-?)?(\d-?)?(\d-?)?(\d-?)?\d?$/g; // +?9(?999)?9-?9-?9-?9-?9-?9-?9
      const match = this.value.match(regex);

      if (this.value !== '' && match === null) {
        this.value = this.prevValue;
      } else {
        if (this.isError) {
          this.isError = this.validation();
        }

        // Проверяем количество цифр
        const length = this.value.replace(/[.\-+(d)]/g, '').length;
        if (length <= 11) {
          this.prevValue = this.value;
        } else {
          this.value = this.prevValue;
        }
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
