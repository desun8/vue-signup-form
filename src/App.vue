<template>
  <div id="app">
    <transition name="fade-appear" mode="out-in">
      <Grateful key="grateful" v-if="isGrateful" :type="type"/>

      <div v-else class="wrapper">
        <Header :is-sign-up="isSignUp" :change-type="changeType"/>
        <transition name="fade-appear" mode="out-in">
        <SignUp v-if="isSignUp" :set-is-grateful="setIsGrateful"/>
        <SignIn v-else :set-is-grateful="setIsGrateful" />
        </transition>
      </div>
    </transition>
  </div>
</template>

<script>
import SignUp from '@/components/SignUp/SignUp';
import Header from '@/components/Header';
import Grateful from '@/components/Grateful';
import SignIn from '@/components/SignIn/SignIn';

export const TYPE_SIGNUP = 'SIGNUP';
export const TYPE_SIGNIN = 'SIGNIN';

export default {
  name: 'App',
  components: {
    SignIn,
    Grateful,
    Header,
    SignUp,
  },

  data() {
    return {
      type: TYPE_SIGNUP,
      isGrateful: false,
    };
  },

  computed: {
    isSignUp() {
      return this.type === TYPE_SIGNUP;
    }
  },

  methods: {
    changeType(value) {
      this.type = value;
    },

    setIsGrateful(newVal) {
      this.isGrateful = newVal;
    }
  }
};
</script>

<style>
*, *::before, *::after {
  box-sizing: border-box;
}

#app {
  --c-blue: #0880ae;
  --c-text: #756f86;
  --font: 'IBM Plex Sans', Avenir, Helvetica, Arial, sans-serif;

  width: min(500px, 100%);
  margin: 0 auto;
  padding: 40px 30px;

  color: #2c2738;
  font-family: var(--font);
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

input {
  font-family: var(--font);
  font-size: inherit;
  appearance: none;
}

.btn {
  padding: 0;
  background: none;
  border: 0;
  font-family: inherit;
  font-size: inherit;
}

.link {
  text-decoration: none;
}

.link,
.btn--link {
  position: relative;
  color: var(--c-blue);
  cursor: pointer;
}

.link::after,
.btn--link::after {
  content: '';
  position: absolute;
  bottom: 2px;
  left: 0;
  right: 0;
  height: 1px;
  background-color: currentColor;
  transform-origin: 0 50%;
  transform: scaleX(0);
  transition: transform 0.2s ease;
}

.link:hover::after,
.btn--link:hover::after {
  transform: scaleX(1);
}

.fade-appear-enter-active, .fade-appear-leave-active {
  transition: all 0.5s;
}
.fade-appear-enter, .fade-appear-leave-to {
  opacity: 0;
  transform: translateY(30px);
}
</style>

<style scoped>
.wrapper {
  display: grid;
  gap: 56px;
}
</style>
