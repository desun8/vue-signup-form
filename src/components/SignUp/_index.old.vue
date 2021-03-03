<template>
  <form action="">
    <Field :id="`${id}-name`" name="Имя">
      <Name :id="`${id}-name`" :value="name.value" :set-has-error="setHasError(name.hasError)"/>
    </Field>
    <Field :id="`${id}-email`" name="Email">
      <Email :id="`${id}-email`"/>
    </Field>
    <Field :id="`${id}-phone`" name="Номер телефона">
      <Phone :id="`${id}-phone`"/>
    </Field>
    <Field :id="`${id}-lang`" name="Язык">
      <Language :id="`${id}-lang`"/>
    </Field>

    <Agreement/>
    <Submit :is-disabled="!isValid"/>
  </form>
</template>

<script>
import Field from '@/components/Field';
import Name from '@/components/SignUp/Name';
import Email from '@/components/SignUp/Email';
import Phone from '@/components/SignUp/Phone';
import Language from '@/components/SignUp/Language';
import Agreement from '@/components/SignUp/Agreement';
import Submit from '@/components/SignUp/Submit';

export default {
  name: 'SignUp',
  components: { Submit, Agreement, Language, Phone, Email, Name, Field },
  data() {
    return {
      id: 'signup',
      isValid: false,
      name: {
        value: '',
        hasError: false
      },
      email: {
        value: '',
        hasError: false
      },
      tel: {
        value: '',
        hasError: false
      },
      lang: {
        value: '',
        hasError: false
      },
      agreement: {
        value: '',
        hasError: false
      },
    };
  },
  methods: {
    setHasError(property) {
      console.log(property);
      return value => {
        property = value;
      };
    }
  }
};
</script>

<style scoped>
form {
  display: grid;
  gap: 34px;
}
</style>
