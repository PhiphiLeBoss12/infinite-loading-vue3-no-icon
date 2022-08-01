<template>
  <div ref="scrollRef">
    <slot></slot>
  </div>
</template>

<script>
import { onMounted, onUnmounted, toRefs } from "vue";
import infiniteScroll from "./composables/infiniteScroll";
import errorHandler from "./composables/errorHandler";

export default {
  props: {
    noResult: {
      type: Boolean,
      default: false,
    },
    message: {
      type: String,
      default: "Failed to load data."
    }
  },

  setup(props, { emit }) {
    const { noResult, message } = toRefs(props);

    const {
      handleScroll,
      scrollRef,
      loading
    } = infiniteScroll(noResult, emit)

    errorHandler(noResult, loading, message);

    onMounted(() => {
			window.addEventListener("scroll", handleScroll)
		})
		onUnmounted(() => {
			window.removeEventListener("scroll", handleScroll)
		})

    return {
      scrollRef,
      loading,
    }
  },
}
</script>
