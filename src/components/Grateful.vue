<template>
  <div class="grateful">
    <span class="grateful__message">{{message}}</span>
  </div>
</template>

<script>
import { TYPE_SIGNIN } from '@/App';

export default {
  name: 'Grateful',
  props: {
    type: String
  },
  computed: {
    message() {
      if (this.type === TYPE_SIGNIN) {
        return 'С возвращением!';
      }

      return 'Спасибо за регистрацию!';
    }
  }
};
</script>

<style scoped>
.grateful {}
.grateful__message {
  font-size: 34px;
}
</style>
