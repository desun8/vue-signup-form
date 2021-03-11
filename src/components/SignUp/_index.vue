<template>
  <form action="/registration" @submit.prevent="handleSubmit">
    <div class="field">
      <label for="name">Имя</label>
      <input id="name"
             v-model.trim="name.value"
             @blur="handleBlur"
             @input="handleInput"
             :class="{'has-error': name.hasError}"
             :name="name.name"
             type="text"
             autocomplete="name"
             placeholder="Введите Ваше имя">
      <ErrorMessage/>
    </div>

    <div class="field">
      <label for="email">Email</label>
      <input id="email"
             v-model.trim="email.value"
             @blur="handleBlur"
             @input="handleInput"
             :class="{'has-error': email.hasError}"
             :name="email.name"
             type="text"
             autocomplete="email"
             placeholder="Введите ваш email">
      <ErrorMessage/>
    </div>

    <div class="field">
      <label for="tel">Номер телефона</label>
      <input id="tel"
             v-model.trim="tel.value"
             @blur="handleBlur"
             @input="handleInput"
             :class="{'has-error': tel.hasError}"
             :name="tel.name"
             type="tel"
             autocomplete="tel"
             placeholder="Введите номер телефона">
      <ErrorMessage/>
    </div>

    <div class="field">
      <label for="lang">Язык</label>
      <Language
          id="lang"
          :set-value="setFieldValue(lang)"
          :check-form="formValidation"
          :name="lang.name"
          :placeholder="'Язык'"/>
      <ErrorMessage/>
    </div>

    <Agreement :change-value="setFieldValue(agreement)" :check-form="formValidation"/>

    <SubmitBtn :is-disabled="!isValid"/>
  </form>
</template>

<script>
/* eslint-disable no-case-declarations */
import Language from '@/components/SignUp/Language';
import Agreement from '@/components/SignUp/Agreement';
import SubmitBtn from '@/components/SignUp/SubmitBtn';
import ErrorMessage from '@/components/SignUp/ErrorMessage';

export default {
  name: 'SignUp',
  components: { ErrorMessage, SubmitBtn, Agreement, Language },
  data() {
    return {
      id: 'signup',
      isValid: false,
      name: {
        name: 'name',
        value: '',
        hasError: false
      },
      email: {
        name: 'email',
        value: '',
        hasError: false
      },
      tel: {
        name: 'tel',
        value: '',
        prevValue: '',
        hasError: false
      },
      lang: {
        name: 'lang',
        value: '',
        hasError: false
      },
      agreement: {
        name: 'agreement',
        value: false,
        hasError: false
      },
    };
  },
  methods: {
    setFieldValue(field) {
      return function (newVal) {
        field.value = newVal;
      };
    },

    formValidation() {
      const fields = [this.name, this.email, this.tel, this.lang, this.agreement];
      const isValid = fields.every(el => el.value !== '' && el.value !== false && el.hasError === false);
      this.isValid = isValid;
    },

    telValidation() {
      const { value } = this.tel;
      const regexp = /^\+?\d[(]?[0-9]{3}[)]?[-0-9]*\d$/g;
      let isMatch = false;
      let isFill = false;

      isMatch = value.match(regexp) !== null;

      // Проверка на наличие парных "(" и ")".
      if (isMatch) {
        const openRoundBrackets = value.search(/\(/) !== -1;
        const closeRoundBrackets = value.search(/\)/) !== -1;
        const hasBrackets = openRoundBrackets && closeRoundBrackets;

        if (!hasBrackets) {
          const onlyOpenBrackets = openRoundBrackets && !closeRoundBrackets;
          const onlyCloseBrackets = !openRoundBrackets && closeRoundBrackets;

          // Если скобка не парная (например забыли), то добавляем.
          if (onlyOpenBrackets) {
            const pos = value.search(/\(/) + 4;
            this.tel.value = value.slice(0, pos) + ')' + value.slice(pos);
          } else if (onlyCloseBrackets) {
            const pos = value.search(/\)/) - 3;
            this.tel.value = value.slice(0, pos) + '(' + value.slice(pos);
          }
        }
      }

      if (isMatch) {
        const numbers = value.replace(/[.\-+(d)]/g, '');
        isFill = numbers.length === 11;
      }

      const hasError = !(isMatch && isFill);
      this.tel.hasError = value !== '' ? hasError : false;
    },

    validation(type) {
      const check = (regexp, property) => {
        const hasError = property.value.match(regexp) === null;
        property.hasError = property.value !== '' ? hasError : false;
      };

      let regexp = '';

      switch (type) {
        case this.name.name:
          regexp = /^[a-zа-я]+(([ -][a-zа-я])?[a-zа-я]*)*$/i;
          check(regexp, this.name);
          break;
        case this.email.name:
          regexp = /(?:[a-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-z0-9!#$%&'*+/=?^_`{|}~-]+)*|"*")@(?:(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?\.)+[a-z0-9](?:[a-z0-9-]*[a-z0-9])?|\[(?:(?:(2(5[0-5]|[0-4][0-9])|1[0-9][0-9]|[1-9]?[0-9]))\.){3}(?:(2(5[0-5]|[0-4][0-9])|1[0-9][0-9]|[1-9]?[0-9])|[a-z0-9-]*[a-z0-9]+)])/igm;
          check(regexp, this.email);
          break;
        case this.tel.name:
          this.telValidation();
          break;
      }

      this.formValidation();
    },

    handleInput(e) {
      const type = e.target.getAttribute('name', '');

      switch (type) {
        case this.name.name:
          if (this.name.hasError) {
            this.validation(type);
          }
          break;
        case this.email.name:
          if (this.email.hasError) {
            this.validation(type);
          }
          break;
        case this.tel.name:
          const { value, prevValue, hasError } = this.tel;
          const regex = /^\+?\d?[(]?\d{0,3}[)]?(\d-?)?(\d-?)?(\d-?)?(\d-?)?(\d-?)?(\d-?)?\d?$/g; // +?9(?999)?9-?9-?9-?9-?9-?9-?9
          const match = value.match(regex);

          if (value !== '' && match === null) {
            this.tel.value = prevValue;
          } else {
            if (hasError) {
              this.validation(type);
            }

            // Проверяем количество цифр
            const length = value.replace(/[.\-+(d)]/g, '').length;
            if (length <= 11) {
              this.tel.prevValue = value;
            } else {
              this.tel.value = prevValue;
            }
          }
      }
    },

    handleBlur(e) {
      const type = e.target.getAttribute('name', '');
      this.validation(type);
    },

    handleSubmit(e) {
      // Проверка перед отправкой
      [this.name, this.email, this.tel].some(input => {
        this.validation(input.name);
        return input.hasError;
      });

      if (this.isValid) {
        console.log('Send form!');

        const inputs = [this.name, this.email, this.tel, this.lang, this.agreement];
        const url = e.target.action || '/registration';
        const data = new FormData();

        inputs.forEach(input => {
          data.append(input.name, input.value);
        });

        fetch(url, { method: 'POST', body: data })
            .then(response => {
              if (!response.ok) throw new Error(response.status);

              return response.json();
            })
            .catch(error => {
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

.field {
  position: relative;
  display: grid;
  gap: 6px;
}

label {
  color: var(--c-text);
  font-weight: 500;
}

label::first-letter {
  text-transform: capitalize;
}

.field >>> input {
  width: 100%;
  padding: 16px;
  border: 1px solid #dbe2ea;
  border-radius: 6px;
  box-shadow: 0 4px 8px rgba(44, 39, 56, 0.04);
}

.field >>> input::placeholder {
  color: #7c9cbf;
}

.field >>> input:focus {
  outline: 0;
  border-color: var(--c-blue);
  box-shadow: 0 4px 8px rgba(44, 39, 56, 0.04), inset 1px 0 0 var(--c-blue), inset 0 1px 0 var(--c-blue), inset -1px 0 0 var(--c-blue), inset 0 -1px 0 var(--c-blue);
}

.field >>> input:-webkit-autofill,
.field >>> input:-webkit-autofill:hover,
.field >>> input:-webkit-autofill:focus {
  background-color: transparent !important;
  color: inherit !important;
  -webkit-box-shadow: 0 0 0 50px white inset;
}
</style>
