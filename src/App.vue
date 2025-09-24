<script setup>
import { ref, onMounted, onUnmounted } from "vue";

const x = ref(0);
const y = ref(0);

const visible = ref(false);

function updateCursor(e) {
  x.value = e.clientX;
  y.value = e.clientY;

  if (
    x.value - 630 > 20
    && x.value - 630 < 205
    && y.value - 260 > 50
    && y.value - 260 < 250
  ) {
    visible.value = true;
  } else {
    visible.value = false;
  }
}

onMounted(() => {
  window.addEventListener("mousemove", updateCursor);
  window.addEventListener("touchmove", updateCursor);
});

onUnmounted(() => {
  window.removeEventListener("mousemove", updateCursor);
  window.removeEventListener("touchmove", updateCursor);
});
</script>

<template>
  <div
    class="
      w-full h-[100vh] relative
      flex items-center justify-center
    "
  >
    <div
      class="
        text-lg scale-[0.2]
      "
    >
      absurd
    </div>
    <div
      class="
        fixed w-[321px] h-[450px]
      "
      :style="{ left: x + 'px', top: y + 'px' }"
      style="transform: translate(-50%, -50%);"
    >
      <img
        class="absolute z-0"
        src="/layer0.png"
        alt="cursor"
      />
      <div
        class="
          absolute text-lg z-1
        "
        :class="{ hidden: !visible }"
        :style="{ left: x - 630 + 'px', top: y - 260 + 'px' }"
      >
        absurd
      </div>
      <img
        class="absolute z-2"
        src="/layer1.png"
        alt="cursor"
      />
    </div>
  </div>
</template>

<style scoped>
  #custom-cursor {
    pointer-events: none; /* blockiert keine Klicks */
  }
</style>
