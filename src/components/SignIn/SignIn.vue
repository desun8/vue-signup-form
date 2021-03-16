<template>
  <form action="/signin" @submit.prevent="handleSubmit">
    <Login :set-login-length="setLoginLength"/>
    <Password :set-password-length="setPasswordLength" />
    <SubmitBtn :is-disabled="!isValid" :type="type" />
  </form>
</template>

<script>
import Login from '@/components/SignIn/Login';
import Password from '@/components/SignIn/Password';
import SubmitBtn from '@/components/SubmitBtn';

import { TYPE_SIGNIN } from '@/App';


export default {
  name: 'SignIn',
  components: { SubmitBtn, Password, Login },
  props: {
    setIsGrateful: {
      type: Function,
      required: true
    }
  },
  data() {
    return {
      type: TYPE_SIGNIN,
      isValid: false,
      loginLength: 0,
      passwordLength: 0
    };
  },
  methods: {
    setLoginLength(val) {
      this.loginLength = val;
    },
    setPasswordLength(val) {
      this.passwordLength = val;
    },

    validation() {
      const fields = [this.loginLength, this.passwordLength];

      this.isValid = fields.every((field) => field > 0);
    },

    handleSubmit(event) {
      if (this.isValid) {
        event.preventDefault();

        const url = event.currentTarget.action || "/signin";
        const data = new FormData(event.currentTarget);

        fetch(url, { method: "POST", body: data })
            .then((response) => {
              if (!response.ok) {
                if (response.status === 401) {
                  alert(
                      "Ошибка! Неверный логин и/или пароль.\nПопробуйте 🐼:\nлогин - hello@world.js\nпароль - root"
                  );
                }
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
  },
  watch: {
    loginLength() {
      this.validation();
    },
    passwordLength() {
      this.validation();
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
