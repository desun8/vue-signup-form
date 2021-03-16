<template>
  <Field id="lang" label="Язык">
    <Dropdown
        id="lang"
        :optionsItems="langItems"
        :change-value="setValue"
        name="lang"
        placeholder="Язык"
    />
  </Field>
</template>

<script>
import Field from '@/components/Field/Field';
import Dropdown from '@/components/SignUp/Dropdown/Dropdown';

export default {
  name: 'Language',
  components: { Dropdown, Field },

  props: {
    setFormValue: {
      type: Function,
      required: true
    }
  },

  data() {
    return {
      value: '',
      langItems: ['Русский', 'Английский', 'Китайский', 'Испанский']
    };
  },

  methods: {
    setValue(newVal) {
      this.value = newVal;
    },
  },

  watch: {
    value(newVal) {
      this.setFormValue(newVal);
    }
  }
};
</script>

<style scoped>

</style>
