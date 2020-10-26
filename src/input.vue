<template>
  <div class="wrapper" :class="{error}">
    <input type="text" :value="value" :disabled="disabled" :readonly="readonly"
           @change="$emit('change', $event)"
           @input="$emit('input', $event)"
           @focus="$emit('focus', $event)"
           @blur="$emit('blur', $event)"
    >
    <template v-if="error">
      <Icon name="error" class="icon"></Icon>
      <span class="message">{{ error }}</span>
    </template>
  </div>
</template>

<script>
import Icon from './icon';

export default {
  components: {Icon},
  name: 'GuluInput',
  props: {
    value: {
      type: String
    },
    disabled: {
      type: Boolean,
      default: false
    },
    readonly: {
      type: Boolean,
      default: false
    },
    error: {
      type: String
    }
  }
}
</script>

<style lang="scss" scoped>
$height: 32px;
$border-color: #999;
$border-color-hover: #666;
$border-radius: 4px;
$font-size: 14px;
$box-shadow-color: rgba(0, 0, 0, .5);
$red: #F1453D;
.wrapper {
  display: inline-flex;
  align-items: center;
  vertical-align: middle;
  font-size: $font-size;

  > :not(:last-child) {
    margin-right: .5em;
  }

  > input {
    height: $height;
    border: 1px solid $border-color;
    border-radius: $border-radius;
    padding: 0 0.5em;
    font-size: inherit;

    &:hover:not([readonly]):not([disabled]) {
      border-color: $border-color-hover;
    }

    &:focus:not([readonly]):not([disabled]) {
      box-shadow: inset 0 1px 3px $box-shadow-color;
      outline: none;
    }

    &[disabled] {
      border-color: #bbb;
      color: #bbb;
      cursor: not-allowed;
    }

    &[readonly] {
      outline: none;
      cursor: default;
    }
  }

  &.error {
    > input {
      border-color: $red;

      &:hover:not([readonly]):not([disabled]) {
        border-color: $red;
      }
    }

    > .icon {
      fill: $red;
    }

    > .message {
      color: $red;
    }
  }
}
</style>