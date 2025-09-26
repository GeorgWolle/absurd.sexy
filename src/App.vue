<script setup>
import { Input } from "postcss";
import { ref, onMounted, onUnmounted } from "vue";

const centerX = ref(window.innerWidth / 2);
const centerY = ref(window.innerHeight / 2);
const x = ref(window.innerWidth / 5);
const y = ref(window.innerHeight / 50);

const leave = ref(false);

const visibleFromLeft = 80;
const visibleFromRight = -145;
const visibleFromTop = 288;
const visibleFromBottom = 70;

const magnifierCenterOffsetX = 145;
const magnifierCenterOffsetY = -65;

const visible = ref(false);

const getCoords = e => {
  if (e.type.startsWith("touch")) {
    const touch = e.touches[0]; // erster Finger
    return { x: touch.clientX - 80, y: touch.clientY - 320 };
  } else {
    return { x: e.clientX, y: e.clientY };
  }
};

const updateCursor = e => {
  const { x: cx, y: cy } = getCoords(e);
  x.value = cx;
  y.value = cy;

  centerX.value = window.innerWidth / 2;
  centerY.value = window.innerHeight / 2;

  if (
    centerX.value - x.value <= visibleFromLeft &&
    centerX.value - x.value >= visibleFromRight &&
    centerY.value - y.value <= visibleFromTop &&
    centerY.value - y.value >= visibleFromBottom
  ) {
    visible.value = true;
  } else {
    visible.value = false;
  }
};

const handleGlobalClick = e => {
  if (visible.value) {
    leave.value = true;
  }
};

function handleTwoFingerTap(e) {
  if (e.touches.length === 2) {
    if (visible.value) {
      leave.value = true;
    }
  }
}

onMounted(() => {
  window.addEventListener("mousemove", updateCursor);
  window.addEventListener("click", handleGlobalClick);
  window.addEventListener("touchmove", updateCursor);
  window.addEventListener("touchstart", handleTwoFingerTap);
});

onUnmounted(() => {
  window.removeEventListener("mousemove", updateCursor);
  window.removeEventListener("click", handleGlobalClick);
  window.removeEventListener("touchmove", updateCursor);
  window.removeEventListener("touchstart", handleTwoFingerTap);
});
</script>

<template>
  <div
    v-if="leave"
    class="
      w-full h-[100vh] relative
      flex items-center justify-center
      text-4xl font-bold
    "
  >
    Bravo!
  </div>
  <div
    v-else
    class="
      w-full h-[100vh] relative
      flex items-center justify-center
    "
  >
    <div
      class="
        flex flex-col items-center justify-center
        text-lg scale-[0.2]
      "
    >
      absurd

      <button
        class="
          px-6 py-3 bg-indigo-600 text-white font-semibold rounded-lg shadow-md
          hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-indigo-400
          focus:ring-offset-2 active:scale-95 transition
        "
      >
        Enter
      </button>
    </div>
    <div
      class="
        fixed w-[325px]
      "
      :style="{ left: x + 'px', top: y + 'px' }"
      style="transform: translate(-50%, -50%); touch-action: none;"
    >
      <img
        class="absolute z-0"
        src="/layer0.png"
        alt="cursor"
      />
      <div
        class="
          absolute flex flex-col items-center justify-center text-lg z-1
        "
        :class="{ hidden: !visible }"
        :style="{ left: magnifierCenterOffsetX + (centerX - x) * .80 + 'px', top: magnifierCenterOffsetY + centerY - y * .80 + 'px' }"
      >
        absurd

        <button
          class="
            px-6 py-3 bg-indigo-600 text-white font-semibold rounded-lg shadow-md
            hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-indigo-400
            focus:ring-offset-2 active:scale-95 transition
          "
        >
          Enter
        </button>
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
    pointer-events: none;
  }
</style>
