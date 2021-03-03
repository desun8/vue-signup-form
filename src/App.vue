<template>
  <div id="app">
    <Header :is-sign-up="isSignUp" :change-type="changeType"/>
    <SignUp v-if="isSignUp"/>
    <div v-else><h2>Форма входа</h2></div>
  </div>
</template>

<script>
import SignUp from '@/components/SignUp/_index';
import Header from '@/components/Header';

export const TYPE_SIGNUP = 'SIGNUP';
export const TYPE_SIGNIN = 'SIGNIN';

export default {
  name: 'App',
  components: {
    Header,
    SignUp,
  },

  data() {
    return {
      type: TYPE_SIGNUP
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

  display: grid;
  gap: 56px;
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
</style>
