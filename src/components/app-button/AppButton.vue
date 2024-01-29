<script lang="ts" setup>
import { ref, computed, withDefaults, defineProps, defineEmits } from 'vue';
import { RouterLinkProps } from 'vue-router';

type Props = {
    variant?: "filled" | "outline";
    size?: "small" | "regular" | "large";
    color?: "green" | "dark-green" | "red" | "orange";
    href?: string;
    to?: RouterLinkProps['to'];
    block?: boolean;
    disabled?: boolean;
};

const props = withDefaults(defineProps<Props>(), {
    variant: "filled",
    size: "regular",
    color: "green",
});

const emit = defineEmits(["click"]);

const variantClass = computed(() => {
    return `btn-variant--${props.variant}`;
})

const sizeClass = computed(() => {
    return `btn-size--${props.size}`;
});

const colorClass = computed(() => {
    return `btn-color--${props.color}`;
});

const btnTag = computed(() => {
    if (props.href && props.to) {
        throw new Error("Button cannot have both to and href props");
    } else if (props.href) {
        return "a";
    } else if (props.to) {
        return "router-link";
    }

    return "button";
});

const onClick = (event: MouseEvent) => {
  if (!props.disabled) {
      emit("click", event);
  }
};
</script>

<template>
  <component
    :is="btnTag"
    :to="to"
    :href="href"
    :disabled="disabled"
    class="btn"
    :class="[
      variantClass,
      sizeClass,
      colorClass,
      {
        'btn--disabled': disabled,
        'btn--block': block
      }
    ]"
    @click="onClick"
  >
    <slot></slot>
  </component>
</template>

<style lang="scss" scoped>
$colorVars: (
  red: (
    main: #F55555,
    hover: #FC6D6D
  ),
  green: (
    main: #30A47A,
    hover: #3DC696
  ),
  orange: (
    main: #FFBF52,
    hover: #FFD788
  ),
  darkGreen: (
    main: #113A29,
    hover: #235840
  ),
);

@mixin btnFilledColor($color) {
  background-color: map-get(map-get($colorVars, $color), "main");
  border-color: map-get(map-get($colorVars, $color), "main");

  &:hover, &:focus {
    background-color: map-get(map-get($colorVars, $color), "hover");
    border-color: map-get(map-get($colorVars, $color), "hover");
  }
}

@mixin btnOutlineColor($color) {
  background-color: transparent;
  border-color: map-get(map-get($colorVars, $color), "main");
  color: map-get(map-get($colorVars, $color), "main");

  &:hover, &:focus {
    background-color: lighten(map-get(map-get($colorVars, $color), "main"), 50%);
    border-color: map-get(map-get($colorVars, $color), "hover");
  }
}

.btn {
  border-width: 1px;
  border-style: solid;
  border-radius: 2rem;
  color: #fff;
  text-decoration: none;
  transition: all 0.5s;
  font-weight: 600;
  display: inline-block;
  min-width: 100px;
  margin: 5px;

  &-size {
    &--small {
      font-size: 14px;
      padding: 10px 16px;
    }

    &--regular {
      font-size: 16px;
      padding: 14px 24px;
    }

    &--large {
      font-size: 18px;
      padding: 16px 24px;
    }
  }

  &-color {
    &--green {
      &.btn-variant {
        &--filled {
          @include btnFilledColor(green);
        }

        &--outline {
          @include btnOutlineColor(green);
        }
      }
    }

    &--dark-green {
      &.btn-variant {
        &--filled {
          @include btnFilledColor(darkGreen);
        }

        &--outline {
          @include btnOutlineColor(darkGreen);
        }
      }
    }

    &--red {
      &.btn-variant {
        &--filled {
          @include btnFilledColor(red);
        }

        &--outline {
          @include btnOutlineColor(red);
        }
      }
    }

    &--orange {
      &.btn-variant {
        &--filled {
          @include btnFilledColor(orange);
          color: #171717;
        }

        &--outline {
          @include btnOutlineColor(orange);
        }
      }
    }
  }

  &--disabled {
    opacity: 0.4;
    pointer-events: none;
  }

  &--block {
    display: block;
  }

  &:hover {
    cursor: pointer;
  }
}
</style>