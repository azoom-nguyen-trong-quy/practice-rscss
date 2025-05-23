<script setup>
const props = defineProps({
  variant: {
    type: String,
    default: '-default',
  },

  //Hiển thị icon hay button
  icon: {
    type: Boolean,
    default: false,
  },

  //Hiển thị icon prepend
  prependIcon: {
    type: Boolean,
    default: false,
  },

  //Hiển thị icon append
  appendIcon: {
    type: Boolean,
    default: false,
  },

  //Đảo chiều icon
  stacked: {
    type: Boolean,
    default: false,
  },
})
</script>

<template>
  <div :class="['button-base', variant]">
    <div v-if="icon" class="icon">
      <img src="@/assets/logo.svg" width="24" height="24px" />
    </div>

    <div
      v-else
      :class="[
        'button-box',
        { '-prepend-icon': prependIcon, '-append-icon': appendIcon, '-stacked': stacked },
      ]"
    >
      <img class="icon -prepend" src="@/assets/logo.svg" width="18" height="18" />
      <div class="text">
        <slot />
      </div>
      <img class="icon -append" src="@/assets/logo.svg" width="18" height="18" />
    </div>
  </div>
</template>

<style lang="scss" scoped>
.button-base {
  grid-area: content;
  border-right: 1px solid #333;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 300px;
  font-size: 14px;

  > .icon {
    width: 48px;
    height: 48px;
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 50%;
  }

  > .button-box {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 8px;
    padding: 10px 16px;
    border-radius: 4px;
    cursor: pointer;
  }

  &.-default > .button-box,
  &.-default > .icon {
    box-shadow: 3px 3px 5px 0px rgba(0, 0, 0, 0.75);
  }

  &.-outlined > .button-box,
  &.-outlined > .icon {
    border: 1px solid #fff;
  }

  &.-tonal > .button-box,
  &.-tonal > .icon {
    background-color: rgb(47, 47, 47);
  }

  &.-plain > .button-box,
  &.-plain > .icon {
    opacity: 0.62;
  }

  > .button-box.-prepend-icon > .icon.-prepend,
  > .button-box.-append-icon > .icon.-append {
    display: block;
  }

  > .button-box.-stacked {
    flex-direction: column;
  }

  > .button-box > .icon {
    display: none;
  }
}
</style>
