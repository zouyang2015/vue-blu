<template>
  <span class="blu-ipt-number control has-addons" :class="{'is-disabled': disabled}">
    <a class="button" :class="[sizeClass]" @click="decrease" v-if="mode==='s'">
      <span class="icon is-small">
        <i class="fa fa-minus"></i>
      </span>
    </a>
    <input class="input" :class="[sizeClass]" type="text" v-model="interVal" @keydown="handleKeyDown">
    <a class="button" :class="[sizeClass]" @click="decrease" v-if="mode!=='s'">
      <span class="icon is-small">
        <i class="fa fa-minus"></i>
      </span>
    </a>
    <a class="button" :class="[sizeClass]" @click="increase">
      <span class="icon is-small">
        <i class="fa fa-plus"></i>
      </span>
    </a>
  </span>
</template>
<script>
export default {
  props: {
    min: {
      type: Number,
      default: 0,
    },
    max: {
      type: Number,
      default: Infinity,
    },
    step: {
      type: Number,
      default: 1,
    },
    disabled: Boolean,
    val: {
      type: Number,
      default: 0,
    },
    onChange: {
      type: Function,
      default() {},
    },
    size: String,
    mode: {
      type: String,
      default: '',
    },
  },

  computed: {
    sizeClass() {
      return this.size ? `is-${this.size}` : null;
    },
  },

  data() {
    return {
      interVal: this.val,
    };
  },

  methods: {
    increase() {
      if (this.max) {
        (Number(this.interVal) + this.step <= this.max) && this.changeVal(this.step);
      } else {
        this.changeVal(this.step);
      }
    },
    decrease() {
      if (this.min || this.min === 0) {
        (Number(this.interVal) - this.step >= this.min) && this.changeVal(-this.step);
      } else {
        this.changeVal(-this.step);
      }
    },
    changeVal(num) {
      if (this.disabled) return;
      this.interVal = Number(this.interVal);
      this.interVal += num;
      this.$emit('input', this.interVal);
      this.onChange(this.interVal);
    },
    handleKeyDown(e) {
      if (e.keyCode === 38) {
        this.increase();
      } else if (e.keyCode === 40) {
        this.decrease();
      }
    },
  },
};
</script>
