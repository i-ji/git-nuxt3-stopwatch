<template>
  <div>
    <button
      @click="timerAction"
      :disabled="disabledBtn"
      class="w-[72px] h-[45px] leading-[45px] bg-gray-300 font-bold"
    >
      {{ btnName }}
    </button>
  </div>
</template>

<script setup lang="ts">
const props = defineProps(["btnName", "disabledBtn"]);
const emits = defineEmits(["timerAction"]);
const timerAction = () => {
  emits("timerAction");
};
</script>
