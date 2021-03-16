<template>
  <Field id="login" label="Имя пользователя">
    <input
        :class="{'has-error': showError}"
        v-model.trim="value"
        @focus="handleFocus"
        @blur="handleBlur"
        id="login"
        name="login"
        autocomplete="user"
        placeholder="Введите ваш email (hello@world.js)"/>
  </Field>
</template>

<script>
import Field from '@/components/Field/Field';

export default {
  name: 'Login',
  components: { Field },
  props: {
    setLoginLength: {
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
    setShouldValid(newVal) {
      this.shouldValid = newVal;
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
      return this.value.length === 0;
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
      this.setLoginLength(newVal.length);
    }
  }
};
</script>

<style scoped>

</style>
