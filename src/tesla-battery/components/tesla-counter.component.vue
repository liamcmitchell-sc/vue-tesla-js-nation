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
          {{ value }}
          <span>{{ unit }}</span>
        </p>
        <div class="tesla-counter__controls" tabindex="-1">
          <button
            tabindex="-1"
            type="button"
            @click="increment"
            :disabled="value === max"
          ></button>
          <button
            tabindex="-1"
            type="button"
            @click="decrement"
            :disabled="value === min"
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
    unit: {
      type: String,
      required: true,
    },
    value: {
      type: Number,
      required: true,
    },
    max: {
      type: Number,
      required: true,
    },
    min: {
      type: Number,
      required: true,
    },
    step: {
      type: Number,
      required: true,
    },
  },
  data: function() {
    return {
      focused: false,
    };
  },
  methods: {
    increment() {
      if (this.value < this.max) {
        this.$emit('change', this.value + this.step);
      }
    },
    decrement() {
      if (this.value > this.min) {
        this.$emit('change', this.value - this.step);
      }
    },
    onFocus(event) {
      this.focused = false;
      event.preventDefault();
      event.stopPropagation();
    },
    onBlur(event) {
      this.focused = true;
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
