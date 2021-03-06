<template>
  <div :class="computedClasses" class="material-input__component">
    <input
      v-if="type === 'email'"
      :name="name"
      :id="id"
      :placeholder="placeholder"
      v-model="valueCopy"
      :readonly="readonly"
      :disabled="disabled"

      :autocomplete="autocomplete"
      :required="required"
      type="email"

      class="material-input"

      @focus="handleFocus(true)"
      @blur="handleFocus(false)"
      @input="handleModelInput"
    >
    <input
      v-if="type === 'url'"
      :name="name"
      :id="id"
      :placeholder="placeholder"
      v-model="valueCopy"
      :readonly="readonly"
      :disabled="disabled"

      :autocomplete="autocomplete"
      :required="required"
      type="url"

      class="material-input"

      @focus="handleFocus(true)"
      @blur="handleFocus(false)"
      @input="handleModelInput"
    >
    <input
      v-if="type === 'number'"
      :name="name"
      :id="id"
      :placeholder="placeholder"
      v-model="valueCopy"
      :readonly="readonly"
      :disabled="disabled"

      :autocomplete="autocomplete"
      :max="max"
      :min="min"

      :step="step"
      :minlength="minlength"
      :maxlength="maxlength"
      :required="required"
      type="number"
      class="material-input"

      @focus="handleFocus(true)"
      @blur="handleFocus(false)"
      @input="handleModelInput"
    >
    <input
      v-if="type === 'password'"
      :name="name"
      :id="id"
      :placeholder="placeholder"
      v-model="valueCopy"
      :readonly="readonly"
      :disabled="disabled"

      :autocomplete="autocomplete"
      :max="max"
      :min="min"

      :step="step"
      :required="required"
      type="password"
      class="material-input"

      @focus="handleFocus(true)"
      @blur="handleFocus(false)"
      @input="handleModelInput"
    >
    <input
      v-if="type === 'tel'"
      :name="name"
      :id="id"
      :placeholder="placeholder"
      v-model="valueCopy"
      :readonly="readonly"
      :disabled="disabled"

      :autocomplete="autocomplete"
      :required="required"
      type="tel"

      class="material-input"

      @focus="handleFocus(true)"
      @blur="handleFocus(false)"
      @input="handleModelInput"
    >
    <input
      v-if="type === 'text'"
      :name="name"
      :id="id"
      :placeholder="placeholder"
      v-model="valueCopy"
      :readonly="readonly"
      :disabled="disabled"
      :autocomplete="autocomplete"
      :minlength="minlength"
      :maxlength="maxlength"

      :required="required"
      type="text"
      class="material-input"

      @focus="handleFocus(true)"
      @blur="handleFocus(false)"
      @input="handleModelInput"
    >

    <span class="material-input-bar"/>

    <label class="material-label">
      <slot/>
    </label>

    <div v-if="errorMessages" class="material-errors">
      <div v-for="error in computedErrors" :key="error" class="material-error">
        {{ error }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'MaterialInput',
  props: {
    autofocus: {
      type: Boolean,
      default: false
    },
    id: {
      type: String,
      default: null
    },
    name: {
      type: String,
      default: null
    },
    type: {
      type: String,
      default: 'text'
    },
    value: {
      type: String,
      default: null
    },
    placeholder: {
      type: String,
      default: null
    },
    readonly: {
      type: Boolean,
      default: false
    },
    disabled: {
      type: Boolean,
      default: false
    },
    min: {
      type: String,
      default: null
    },
    max: {
      type: String,
      default: null
    },
    step: {
      type: Number,
      default: null
    },
    minlength: {
      type: Number,
      default: null
    },
    maxlength: {
      type: Number,
      default: null
    },
    required: {
      type: Boolean,
      default: true
    },
    autocomplete: {
      type: String,
      default: null
    },
    errorMessages: {
      type: [Array, String],
      default: null
    }
  },
  data() {
    return {
      valueCopy: null,
      focus: false,
      valid: true
    }
  },
  computed: {
    computedErrors() {
      return typeof this.errorMessages === 'string'
        ? [this.errorMessages] : this.errorMessages
    },
    computedClasses() {
      return {
        'material--active': this.focus,
        'material--disabled': this.disabled,
        'material--has-errors': Boolean(
          !this.valid ||
            (this.errorMessages && this.errorMessages.length)),
        'material--raised': Boolean(
          this.focus ||
            this.valueCopy || // has value
            (this.placeholder && !this.valueCopy)) // has placeholder
      }
    }
  },
  watch: {
    value(newValue) {
      // This watch works from the code side of the 2-way-binding:
      this.copyValue(newValue)
    }
  },
  beforeMount() {
    // Here we are following the Vue2 convention on custom v-model:
    // https://github.com/vuejs/vue/issues/2873#issuecomment-223759341
    this.copyValue(this.value)
  },
  mounted() {
    this.$nextTick(() => {
      if (this.id && this.autofocus) {
        setTimeout(() => {
          document.querySelector('#' + this.id).focus()
        }, 80)
      }
    })
  },
  methods: {
    handleModelInput(event) {
      this.$emit('input', event.target.value, event)
      this.handleValidation()
    },
    handleFocus(focused) {
      this.focus = focused
    },
    handleValidation() {
      this.valid = this.$el ? this.$el.querySelector(
        '.material-input').validity.valid : this.valid
    },
    copyValue(value) {
      this.valueCopy = value
      this.handleValidation()
    }
  }
}
</script>

<style lang="scss">
  // Fonts:
  $font-size-base: 16px;
  $font-size-small: 14px;
  $font-size-smallest: 12px;
  $font-weight-normal: normal;
  // Utils
  $spacer: 10px;
  $transition: 0.2s ease all;
  // Base clases:
  %base-bar-pseudo {
    content: '';
    height: 1px;
    width: 0;
    bottom: 0;
    position: absolute;
    transition: $transition;
  }
  // Mixins:
  @mixin slided-top() {
    top: -2 * $spacer;
    font-size: $font-size-small;
  }
  // Component:
  .material-input__component {
    margin-top: 45px;
    position: relative;
    * {
      box-sizing: border-box;
    }
    .material-input {
      font-size: $font-size-base;
      padding: $spacer $spacer $spacer $spacer / 2;
      display: block;
      width: 100%;
      border: none;
      border-radius: 0;
      &:focus {
        outline: none;
        border: none;
        border-bottom: 1px solid transparent; // fixes the height issue
      }
    }
    .material-label {
      font-size: $font-size-base;
      font-weight: $font-weight-normal;
      position: absolute;
      pointer-events: none;
      left: 0;
      // top: $spacer;
      top: 5px;
      transition: $transition;
    }
    .material-input-bar  {
      position: relative;
      display: block;
      width: 100%;
      &:before {
        @extend %base-bar-pseudo;
        left: 50%;
      }
      &:after {
        @extend %base-bar-pseudo;
        right: 50%;
      }
    }
    // Disabled state:
    &.material--disabled {
      .material-input {
        border-bottom-style: dashed;
      }
    }
    // Raised state:
    &.material--raised {
      .material-label {
        @include slided-top();
      }
    }
    // Active state:
    &.material--active {
      .material-input-bar {
        &:before,
        &:after {
          width: 50%;
        }
      }
    }
    // Errors:
    .material-errors {
      position: relative;
      overflow: hidden;
      .material-error {
        font-size: $font-size-smallest;
        line-height: $font-size-smallest + 2px;
        overflow: hidden;
        margin-top: 0;
        padding-top: $spacer / 2;
        padding-right: $spacer / 2;
        padding-left: 0;
      }
    }
  }
  // Theme:
  $color-white: white;
  $color-grey: #9E9E9E;
  $color-grey-light: #E0E0E0;
  $color-blue: #2196F3;
  $color-red: #F44336;
  $color-black: black;
  .material-input__component {
    background: $color-white;
    .material-input {
      background: none;
      color: $color-black;
      border-bottom: 1px solid $color-grey-light;
    }
    .material-label {
      color: $color-grey;
    }
    .material-input-bar {
      &:before,
      &:after {
        background: $color-blue;
      }
    }
    // Active state:
    &.material--active {
      .material-label {
        color: $color-blue;
      }
    }
    // Errors:
    &.material--has-errors {
      // These styles are required
      // for custom validation:
      &.material--active .material-label {
        color: $color-red;
      }
      .material-input-bar {
        &:before,
        &:after {
          background: $color-red;
        }
      }
      .material-errors {
        color: $color-red;
      }
    }
  }
</style>
