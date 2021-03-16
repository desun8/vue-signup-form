<template>
  <Field id="email" label="Email">
    <input
        :class="{'has-error': showError}"
        v-model.trim="value"
        @focus="handleFocus"
        @blur="handleBlur"
        id="email"
        type="email"
        name="email"
        autoComplete="email"
        placeholder="Введите Ваш email"
    />
  </Field>
</template>

<script>
import Field from '@/components/Field/Field';

export default {
  name: 'Email',
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
      const regexp = /(?:[a-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-z0-9!#$%&'*+/=?^_`{|}~-]+)*|"*")@(?:(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?\.)+[a-z0-9](?:[a-z0-9-]*[a-z0-9])?|\[(?:(?:(2(5[0-5]|[0-4][0-9])|1[0-9][0-9]|[1-9]?[0-9]))\.){3}(?:(2(5[0-5]|[0-4][0-9])|1[0-9][0-9]|[1-9]?[0-9])|[a-z0-9-]*[a-z0-9]+)])/gim;
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
      this.setFormValue(newVal, this.hasError)
    }
  }
};
</script>

<style scoped>

</style>
