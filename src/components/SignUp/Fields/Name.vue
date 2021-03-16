<template>
  <Field id="name" label="Имя">
    <input
        :class="{'has-error': showError}"
        v-model.trim="value"
        @focus="handleFocus"
        @blur="handleBlur"
        id="name"
        name="name"
        autoComplete="name"
        placeholder="Введите Ваше имя"
    />
  </Field>
</template>

<script>
import Field from '@/components/Field/Field';

export default {
  name: 'Name',
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
      shouldValid: false
    };
  },

  methods: {
    setValue(newVal) {
      this.value = newVal;
    },
    setShouldValid(newVal) {
      this.shouldValid = newVal;
    },

    validation(value) {
      const regexp = /^[a-zа-я]+(([ -][a-zа-я])?[a-zа-я]*)*$/i;
      const isMatch = value.match(regexp) !== null;
      const hasError = value !== '' ? !isMatch : false;

      return hasError;
    },

    handleFocus() {
      if (this.hasError === false) {
        this.setShouldValid(false);
      }
    },

    handleBlur() {
      this.setShouldValid(true);
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
