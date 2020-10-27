<template>
  <div class="col" :class="colClass" :style="colStyle">
    <slot></slot>
  </div>
</template>

<script>
let validator = (value) => {
  let keys = Object.keys(value);
  let valid = true;
  keys.forEach(key => {
    if (!['span', 'offset'].includes(key)) {
      valid = false;
    }
  });
  return valid;
};
export default {
  name: "GuluCol",
  props: {
    span: {
      type: [Number, String]
    },
    offset: {
      type: [Number, String]
    },
    ipad: {
      type: Object,
      validator
    },
    narrowPc: {
      type: Object,
      validator
    },
    pc: {
      type: Object,
      validator
    },
    widePc: {
      type: Object,
      validator
    }
  },
  data() {
    return {
      gutter: 0
    }
  },
  methods: {
    createClasses(obj, infix = '') {
      if (!obj) {
        return [];
      }
      let classes = [];
      if (obj.span) {
        classes.push(`col-${infix}${obj.span}`);
      }
      if (obj.offset) {
        classes.push(`offset-${infix}${obj.offset}`);
      }
      return classes;
    }
  },
  computed: {
    colClass() {
      let {span, offset, ipad, narrowPc, pc, widePc} = this;
      let createClasses = this.createClasses;
      return [
        ...createClasses({span, offset}),
        ...createClasses(ipad, 'ipad-'),
        ...createClasses(narrowPc, 'narrow-pc-'),
        ...createClasses(pc, 'pc-'),
        ...createClasses(widePc, 'wide-pc-'),
      ];
    },
    colStyle() {
      let {gutter} = this;
      return {
        paddingLeft: gutter / 2 + 'px',
        paddingRight: gutter / 2 + 'px'
      }
    }
  }
}
</script>

<style scoped lang="scss">
.col {
  flex: 1;

  @mixin createColAndOffset($infix: '') {
    $class-prefix: col-;
    @for $n from 1 through 24 {
      &.#{$class-prefix}#{$infix}#{$n} {
        width: $n / 24 * 100%;
        flex: initial;
      }
    }

    $class-prefix: offset-;
    @for $n from 1 through 24 {
      &.#{$class-prefix}#{$infix}#{$n} {
        margin-left: $n / 24 * 100%;
      }
    }
  }

  @include createColAndOffset();

  @media (min-width: 576px) {
    @include createColAndOffset('ipad-');
  }

  @media (min-width: 768px) {
    @include createColAndOffset('narrow-pc-');
  }

  @media (min-width: 992px) {
    @include createColAndOffset('pc-');
  }

  @media (min-width: 1200px) {
    @include createColAndOffset('wide-pc-');
  }
}
</style>