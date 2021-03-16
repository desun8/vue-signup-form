<template>
  <div>
    <transition name="fade-appear" mode="out-in">
      <header key="header-signup" v-if="isSignUp">
        <h1 class="title">Регистрация</h1>
        <span class="message">Уже есть аккаунт? <button class="btn btn--link" type="button"
                                                        @click="onClick">Войти</button></span>
      </header>

      <header key="header-signin" v-else>
        <h1 class="title">Вход</h1>
        <span class="message">Нет аккаунта? <button class="btn btn--link" type="button" @click="onClick">Зарегистрироваться</button></span>
      </header>
    </transition>
  </div>
</template>

<script>
import { TYPE_SIGNUP, TYPE_SIGNIN } from '@/App';

export default {
  name: 'Header',
  props: {
    isSignUp: {
      type: Boolean,
      required: true
    },
    changeType: {
      type: Function,
      required: true
    }
  },

  methods: {
    onClick() {
      const newType = this.isSignUp ? TYPE_SIGNIN : TYPE_SIGNUP;
      this.changeType(newType);
    }
  }
};
</script>

<style scoped>
.title {
  margin-bottom: 8px;
  font-size: 34px;
}

.message {
  line-height: 1.4;
}
</style>
