<template>
  <div class="c-checkbox">
    <input
        v-model="checked"
        @change="handleChange"
        id="agreement"
        class="sr-only"
        type="checkbox">
    <span class="c-checkbox__checkmark">
        <svg width="16" height="12" fill="none">
          <path class="path"
                d="M14.634.634a.8.8 0 011.132 1.132l-9.6 9.6a.8.8 0 01-1.132 0l-4-4a.8.8 0 011.132-1.132L5.6 9.67 14.634.634z"
                fill="#000"/>
        </svg>
      </span>
    <label class="c-checkbox__label" for="agreement">

      <span>Принимаю <a class="link" href="https://cutt.ly/hluD6lM" target="_blank"
                        title="Статья 9. Согласие субъекта персональных данных на обработку его персональных данных"
                        aria-label="Статья 9. Согласие субъекта персональных данных на обработку его персональных данных">условия</a>
      использования</span>
    </label>
  </div>
</template>

<script>
export default {
  name: 'Agreement',
  props: {
    changeValue: Function,
    checkForm: Function,
  },
  data() {
    return { checked: '' };
  },
  methods: {
    handleChange() {
      this.changeValue(this.checked);
      this.checkForm();
    }
  }
};
</script>

<style scoped>
.sr-only {
  position: absolute;
  clip: rect(1px, 1px, 1px, 1px);
  padding: 0;
  border: 0;
  height: 1px;
  width: 1px;
  overflow: hidden;
}

.c-checkbox {
  position: relative;
  display: flex;
  align-items: center;
  color: var(--c-text);
  font-weight: 500;
}

.c-checkbox__checkmark {
  position: absolute;
  top: calc(50% - (28px / 2));
  left: 0;

  display: flex;
  justify-content: center;
  align-items: center;
  width: 28px;
  height: 28px;

  border: 1px solid #dbe2ea;
  border-radius: 4px;
  box-shadow: 0 4px 8px rgba(44, 39, 56, 0.04);
  transition: border-color 0.3s ease;
}

.c-checkbox__label {
  position: relative;
  padding-left: calc(28px + 8px);
  cursor: pointer;
  z-index: 2;
}

.c-checkbox__checkmark .path {
  fill: #fff;
  transition: all 0.2s ease-out;
}

.c-checkbox input:checked + .c-checkbox__checkmark .path {
  fill: var(--c-blue);
}

.c-checkbox input:focus:not(:focus-visible) {
  outline: none;
}

.c-checkbox input:checked + .c-checkbox__checkmark,
.c-checkbox input:focus + .c-checkbox__checkmark,
.c-checkbox__label:hover .c-checkbox__checkmark {
  border-color: var(--c-blue);
}
</style>
