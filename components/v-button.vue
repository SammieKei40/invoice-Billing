<template>
  <button
    :class="[__class, { 'disabled-button': disabled }]"
    class="flex justify-center"
    style="white-space: nowrap"
    :disabled="disabled"
  >
    <component
      v-if="icon || loading"
      :class="`${!iconOnly ? 'mr-2' : ''} my-auto`"
      size="17"
      :color="types[type]"
      :is="loading ? Spinner : icon"
    ></component>
    <span
      class="my-auto"
      :class="!icon && !loading ? (round ? ' mt-1 ml-1 z-50' : 'm-auto') : ''"
    >
      <slot></slot>
    </span>
  </button>
</template>

<script lang="ts" setup>
import Spinner from "../components/svgs/loader-icon";

import { computed } from "@vue/reactivity";

let props = defineProps({
  size: {
    type: String,
    default: "medium",
  },
  type: {
    type: String,
    default: "pry",
  },
  round: {
    type: Boolean,
    default: false,
  },
  icon: {
    type: Object,
  },
  iconOnly: {
    type: Boolean,
    defult: false,
  },
  full: {
    type: Boolean,
    default: false,
  },
  disabled: {
    type: Boolean,
    default: false,
  },
  loading: {
    type: Boolean,
    default: false,
  },
});

let base = "rounded-xl";
let sizes = {
  small: "py-4 px-4",
  medium: "py-4 px-8",
  big: "py-4 px-8 txt-s-4",
};
let types = {
  sec: {
    __class: " border-pry rounded mt-10 bg-blue-500 text-[#fff] font-bold btn-pry",
    text: "#fff",
  },
  pry: {
    __class:
      "text-white bg-[#8E7AE3]  rounded border border-gray-200 text-sm font-medium px-8 py-2.5",
    text: "#fff",
  },
  delete: {
    __class: "text-white bg-red-600 hover:bg-red-700  rounded-lg",
    text: "#fff",
  },
};
// hover:bg-pry-dark hover:text-white transition-colors duration-500

let __class = computed(() => {
  let __base =
    ((types[props.type] && types[props.type].__class) || types["pry"].__class) +
    " " +
    (sizes[props.size] || sizes["medium"]);
  if (props.round) __base += " rounded-full h-12 w-12 max-w-12 center";
  else __base += " " + base;

  if (props.full) __base += " w-full";
  if (props.disabled) __base += " fade-40 cursor-not-allowed";

  return __base;
});
</script>

<style scoped>
button {
  cursor: pointer;
}
.disabled-button {
  cursor: not-allowed;
  opacity: 0.5;
}
</style>
