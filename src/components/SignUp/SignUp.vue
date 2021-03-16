<template>
  <form action="/registration" @submit.prevent="handleSubmit">
    <Name :set-form-value="setFieldValue(name)"/>
    <Email :set-form-value="setFieldValue(email)"/>
    <Phone :set-form-value="setFieldValue(tel)"/>
    <Language :set-form-value="setFieldValue(lang)"/>
    <Agreement :set-form-value="setFieldValue(agreement)"/>

    <SubmitBtn :is-disabled="!isValid" :type="type"/>
  </form>
</template>

<script>
import Agreement from '@/components/SignUp/Fields/Agreement';
import SubmitBtn from '@/components/SubmitBtn';
import Name from '@/components/SignUp/Fields/Name';
import Email from '@/components/SignUp/Fields/Email';
import Phone from '@/components/SignUp/Fields/Phone';
import Language from '@/components/SignUp/Fields/Language';

import { TYPE_SIGNUP } from '@/App';

export default {
  name: 'SignUp',
  components: { Language, Phone, Email, Name, SubmitBtn, Agreement },
  props: {
    setIsGrateful: {
      type: Function,
      required: true
    }
  },
  data() {
    return {
      type: TYPE_SIGNUP,
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
      },
      agreement: {
        value: false,
      },
    };
  },
  methods: {
    validation() {
      const fields = [this.name, this.email, this.tel, this.lang, this.agreement];

      const isValid = fields.every((field) => {
        if (field.hasError !== undefined) {
          return field.value !== '' && field.hasError === false;
        } else {
          return !!field.value;
        }
      });

      this.isValid = isValid;
    },
    setFieldValue(field) {
      return (newVal, hasError) => {
        field.value = newVal;

        if (hasError !== undefined) {
          field.hasError = hasError;
        }

        this.validation();
      };
    },

    handleSubmit(event) {
      if (this.isValid) {
        event.preventDefault();

        const url = event.currentTarget.action || '/registration';
        const data = new FormData(event.currentTarget);

        if (data.has("agreement")) {
          data.set("agreement", `${this.agreement.value}`);
        }

        fetch(url, { method: 'POST', body: data })
            .then((response) => {
              if (!response.ok) {
                throw new Error(`${response.status}`);
              }

              this.setIsGrateful(true);
              return response.json();
            })
            .catch((error) => {
              console.error(error);
            });
      }
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
