<template>
  <div
      ref="container"
      class="dropdown-wrapper"
      @focusin="openDropdown"
      @keydown="handleKeydown"
  >
    <input
        v-model="value"
        ref="input"
        :id="id"
        :class="{'is-active': isActive}"
        :name="name"
        :placeholder="placeholder"
        type="text"
        readonly
    >
    <ul ref="dropdown" class="dropdown" v-show="isActive">
      <li
          class="dropdown__item"
          v-for="item in optionsItems"
          :key="item"
          @click="handleClick"
          tabindex="-1"
      >{{ item }}</li>
    </ul>
  </div>
</template>

<script>
export default {

  name: 'Dropdown',
  props: {
    optionsItems: {
      type: Array,
      required: true
    },
    id: String,
    name: String,
    changeValue: Function,
    placeholder: String,
  },
  data() {
    return {
      value: '',
      isActive: false,
      currentFocus: 0,
    };
  },

  methods: {
    closeDropdown() {
      this.isActive = false;
    },

    openDropdown() {
      this.isActive = true;
    },

    updateValue(newVal) {
      this.value = newVal.trim();
      this.changeValue(this.value);
    },

    handleClick(event) {
      this.updateValue(event.currentTarget.textContent);
    },

    /**
     * Навигация с помощью клавиатуры
     */
    handleKeydown(event) {
      const input = this.$refs.input;
      const dropdownItems = this.$refs.dropdown.children;

      let nextActiveElement = null;
      switch (event.key) {
        case 'ArrowDown':
          if (dropdownItems.length) {
            event.preventDefault();
            this.currentFocus =
                this.currentFocus < dropdownItems.length ? this.currentFocus + 1 : this.currentFocus;

            nextActiveElement = dropdownItems[this.currentFocus - 1];

            nextActiveElement.focus();
          }
          break;
        case 'ArrowUp':
          if (dropdownItems.length) {
            event.preventDefault();

            this.currentFocus = this.currentFocus !== 0 ? this.currentFocus - 1 : 0;

            nextActiveElement =
                this.currentFocus === 0
                    ? input
                    : dropdownItems[this.currentFocus - 1];

            nextActiveElement.focus();
          }
          break;
        case 'Enter':
          if (document.activeElement?.classList.contains('dropdown__item')) {
            event.preventDefault();

            this.updateValue(document.activeElement.textContent);

            this.closeDropdown();
            this.currentFocus = 0;
          }
          break;
        case 'Escape':
        case 'Tab':
          this.closeDropdown();
          this.currentFocus = 0;
          break;
        default:
          break;
      }
    },
  },

  mounted() {
    document.addEventListener('click', (event) => {
      if (this.isActive) {
        if (event.target !== this.$refs.input) {
          this.closeDropdown();
        }
      }
    });
  }
};
</script>

<style scoped>
ul {
  margin: 0;
  padding: 0;
  list-style: none;
}

input {
  cursor: pointer;
}
input.is-active {
  outline: var(--c-blue) auto 2px;
  border-color: var(--c-blue);
}

.dropdown-wrapper {
  position: relative;
}

.dropdown-wrapper::after {
  content: url("arrow-down.svg");
  position: absolute;
  top: calc(50% - (30px / 2));
  right: 10px;
  display: block;
  width: 30px;
  height: 30px;
  cursor: pointer;
}

.dropdown {
  position: absolute;
  top: calc(100% + 4px);
  left: 0;
  right: 0;
  padding: 12px 0;
  background-color: #fff;
  border: 1px solid #dbe2ea;
  box-shadow: 0 4px 8px rgba(44, 39, 56, 0.04), 0 20px 20px rgba(44, 39, 56, 0.04);
  border-radius: 6px;
  z-index: 10;
}

.dropdown__item {
  padding: 12px 15px;
  transition: background-color 0.2s;
  cursor: pointer;
}

.dropdown__item:hover,
.dropdown__item:focus {
  background-color: #ebf4f8;
}

.dropdown__item:focus {
  outline: 0;
}
</style>
