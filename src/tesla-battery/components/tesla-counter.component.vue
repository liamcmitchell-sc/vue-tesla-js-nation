<template>
  <div class="tesla-counter">
    <p class="tesla-counter__title">{{ title }}</p>
    <div class="tesla-counter__container cf">
      <div
        class="tesla-counter__item"
        tabindex="0"
        @blur="onBlur"
        @keydown="onKeyUp"
        @focus="onFocus"
      >
        <p class="tesla-counter__number">
          {{ value.value }}
          <span>°</span>
        </p>
        <div class="tesla-counter__controls" tabindex="-1">
          <button
            tabindex="-1"
            type="button"
            @click="increment"
            :disabled="value.value === value.max"
          ></button>
          <button
            tabindex="-1"
            type="button"
            @click="decrement"
            :disabled="value.value === value.min"
          ></button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'tesla-stats',
  props: {
    title: {
      type: String,
      required: true,
    },
    value: {
      type: Object,
      required: true,
    },
  },
  methods: {
    increment() {
      if (this.value.value < this.value.max) {
        this.value.value = this.value.value + this.value.step;
      }
    },
    decrement() {
      if (this.value.value > this.value.min) {
        this.value.value = this.value.value - this.value.step;
      }
    },
    onFocus(event) {
      this.value.focused = false;
      event.preventDefault();
      event.stopPropagation();
    },
    onBlur(event) {
      this.value.focused = true;
      event.preventDefault();
      event.stopPropagation();
    },
    onKeyUp(event) {
      let handlers = {
        ArrowDown: () => this.decrement(),
        ArrowUp: () => this.increment(),
      };

      if (handlers[event.code]) {
        handlers[event.code]();
        event.preventDefault();
        event.stopPropagation();
      }
    },
  },
};
</script>
