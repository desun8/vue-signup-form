<template>
  <Dropdown
      :items="langItems"
      :change-value="setValue"
      :check-form="checkForm"
      :id="id"
      :name="name"
      :placeholder="placeholder"
  />
</template>

<script>
import Dropdown from '@/components/SignUp/Dropdown/Dropdown';

export default {
  name: 'Language',
  components: { Dropdown },
  props: {
    id: {
      type: String,
      required: true
    },
    setValue: {
      type: Function,
      required: true
    },
    name: String,
    placeholder: String,
    checkForm: Function
  },
  data() {
    return {
      langItems: ['Русский', 'Английский', 'Китайский', 'Испанский']
    };
  }
};
</script>
