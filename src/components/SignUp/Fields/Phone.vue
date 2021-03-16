<template>
  <Field id="phone" label="Номер телефона">
    <input
        :class="{'has-error': showError}"
        v-model.trim="value"
        @input="handleInput"
        @focus="handleFocus"
        @blur="handleBlur"
        id="phone"
        type="tel"
        name="phone"
        autoComplete="tel"
        placeholder="Введите номер телефона"
    />
  </Field>
</template>

<script>
import Field from '@/components/Field/Field';

export default {
  name: 'Phone',
  components: { Field },

  props: {
    setFormValue: {
      type: Function,
      required: true
    }
  },

  data() {
    return {
      value: '',
      prevValue: '',
      shouldValid: false
    };
  },

  methods: {
    setValue(newVal) {
      this.value = newVal;
    },
    setPrevValue(newVal) {
      this.prevValue = newVal;
    },
    setShouldValid(newVal) {
      this.shouldValid = newVal;
    },

    validation(value) {
      const regexp = /^\+?\d[(]?[0-9]{3}[)]?[-0-9]*\d$/g;
      let isMatch = value.match(regexp) !== null;
      let isFill = false;

      if (isMatch) {
        const numbers = value.replace(/[.\-+(d)]/g, '');
        isFill = numbers.length === 11;
      }

      const isError = !(isMatch && isFill);
      const hasError = value !== '' ? isError : false;

      return hasError;
    },

    /**
     * Проверка на наличие парных "(" и ")".
     * Если скобка не парная (например забыли), то добавляем.
     */
    fixBrackets(value, setValue) {
      const openRoundBracket = value.search(/\(/) !== -1;
      const closeRoundBracket = value.search(/\)/) !== -1;

      const onlyOpenBracket = openRoundBracket && !closeRoundBracket;
      const onlyCloseBracket = !openRoundBracket && closeRoundBracket;

      const isLostBrackets = onlyOpenBracket || onlyCloseBracket;

      if (isLostBrackets) {
        let newVal = value;
        let pos = 0;

        if (onlyOpenBracket) {
          pos = value.search(/\(/) + 4;
          newVal = value.slice(0, pos) + ')' + value.slice(pos);
        } else {
          pos = value.search(/\)/) - 3;
          newVal = value.slice(0, pos) + '(' + value.slice(pos);
        }

        setValue(newVal);
      }
    },

    handleInput(event) {
      const { value } = event.currentTarget;
      const regex = /^\+?\d?[(]?\d{0,3}[)]?(\d-?)?(\d-?)?(\d-?)?(\d-?)?(\d-?)?(\d-?)?\d?$/g; // +?9(?999)?9-?9-?9-?9-?9-?9-?9
      const isMatch = value.match(regex) !== null;

      if (value !== '' && isMatch === false) {
        this.setValue(this.prevValue);
      } else {
        // Проверяем количество цифр
        const length = value.replace(/[.\-+(d)]/g, '').length;
        if (length <= 11) {
          this.setPrevValue(value);
          this.setValue(value);
        }
      }
    },

    handleFocus() {
      if (this.hasError === false) {
        this.setShouldValid(false);
      }
    },

    handleBlur() {
      this.setShouldValid(true);
      this.fixBrackets(this.value, this.setValue);
      this.setPrevValue(this.value);
    }
  },

  computed: {
    hasError() {
      return this.validation(this.value);
    },

    showError() {
      if (this.hasError && this.shouldValid) {
        return true;
      } else {
        this.setShouldValid(false);
        return false;
      }
    }
  },
  watch: {
    value(newVal) {
      this.setFormValue(newVal, this.hasError);
    }
  }
};
</script>

<style scoped>

</style>
