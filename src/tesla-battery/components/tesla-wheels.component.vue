<template>
  <div class="tesla-wheels">
    <p class="tesla-wheels__title">Wheels</p>
    <div class="tesla-wheels__container cf">
      <label
        v-for="size of wheels.sizes"
        :key="size"
        :class="[
          {
            'tesla-wheels__item--active': wheels.value === size,
            'tesla-wheels__item--focused': wheels.focused === size,
            'tesla-wheels__item': true,
          },
          `tesla-wheels__item--${size}`,
        ]"
      >
        <input
          type="radio"
          name="wheelsize"
          :value="size"
          @blur="onBlur()"
          @click="changeSize(size)"
          @focus="onFocus(size)"
          :checked="wheels.value === size"
        />
        <p>{{ size }}"</p>
      </label>
    </div>
  </div>
</template>

<script>
export default {
  name: 'tesla-wheels',
  props: {
    wheels: {
      type: Object,
      required: true,
    },
  },
  methods: {
    changeSize(size) {
      this.wheels.value = size;
    },
    onBlur() {
      this.wheels.focused = null;
    },
    onFocus(size) {
      this.wheels.focused = size;
    },
  },
};
</script>
