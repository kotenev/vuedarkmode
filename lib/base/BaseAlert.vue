<!-- *************************************************************************
     TEMPLATE
     ************************************************************************* -->

<template lang="pug">
div(
  :class=`[
    "dm-base-alert",
    "dm-base-alert--" + color
  ]`
)
  base-icon(
    v-if="icon"
    :name="icon"
    class="dm-base-alert__icon dm-base-alert__icon--left"
    size="20px"
  )
  span(
    v-if="$slots.default && $slots.default[0].text.trim()"
    class="dm-base-alert__slot"
  ): slot

  base-icon(
    v-if="closable"
    @click="onClose"
    @keypress="onTabKeypress"
    :clickable="true"
    class="dm-base-alert__icon dm-base-alert__icon--right"
    name="close"
    size="20px"
    tabindex="0"
  )
</template>

<!-- *************************************************************************
     SCRIPT
     ************************************************************************* -->

<script>
// PROJECT
import BaseIcon from "./BaseIcon.vue";

export default {
  components: {
    BaseIcon
  },

  props: {
    closable: {
      type: Boolean,
      default: true
    },
    color: {
      type: String,
      default: "blue",
      validator(x) {
        return ["black", "blue", "green", "orange", "red", "white"].includes(x);
      }
    },
    icon: {
      type: String,
      default: null
    }
  },

  methods: {
    // --> EVENT LISTENERS <--

    onClose(event) {
      this.$emit("close", event);
    },

    onTabKeypress(id, event) {
      event.preventDefault();

      if (event.which === 32) {
        event.target.click();
      }
    }
  }
};
</script>

<!-- *************************************************************************
     STYLE
     ************************************************************************* -->

<style lang="scss">
// IMPORTS
@import "assets/settings/_settings.colors.scss";

// VARIABLES
$c: ".dm-base-alert";
$colors: black, blue, green, red, orange, white;

#{$c} {
  display: flex;
  align-items: center;
  padding: 14px 20px;
  color: $white;
  text-align: left;
  font-family: "Heebo", "Helvetica Neue", Source Sans Pro, Helvetica, Arial,
    sans-serif;
  transition: all 200ms ease-in-out;

  #{$c}__icon {
    flex: 0 0 auto;

    &--left {
      margin-right: 20px;
    }

    &--right {
      margin-left: 20px;
      outline: 0;
      border-radius: 100%;
      transition: all 200ms ease-in-out;

      &:focus {
        transition: box-shadow ease-in-out 0s;
      }
    }
  }

  #{$c}__slot {
    flex: 1;
    font-size: 16px;
    line-height: 22px;
  }

  // --> COLORS <--

  @each $color in $colors {
    &--#{$color} {
      background-color: map-get($mainColors, $color);

      @if ($color == white) {
        color: $oxford-blue;
      }

      #{$c}__icon--right {
        @if ($color == red) {
          &:hover {
            color: $oxford-blue !important;
          }

          &:focus {
            box-shadow: 0 0 0 2px map-get($mainColors, $color),
              0 0 0 3px $oxford-blue;
            color: $oxford-blue !important;
          }
        } @else {
          &:hover {
            color: $crimson !important;
          }

          &:focus {
            box-shadow: 0 0 0 2px map-get($mainColors, $color),
              0 0 0 3px $crimson;
            color: $crimson !important;
          }
        }
      }
    }
  }
}
</style>
