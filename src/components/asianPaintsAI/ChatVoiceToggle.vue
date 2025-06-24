<template>
  <div class="chat-voice-toggle">
    <div
      v-for="button in buttons"
      :key="button.id"
      :class="activeMode === button.id ? 'active-button' : 'inactive-button'"
      @click="setActiveMode(button.id)"
    >
      <span class="text">{{ button.text }}</span>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, computed } from 'vue';

const props = defineProps({
  buttons: {
    type: Array,
    default: () => [
      { id: 'chat', text: 'Chat' },
      { id: 'voice', text: 'Voice' },
    ],
  },
});

const activeMode = ref(props.buttons[0]?.id || 'chat'); // Default to first button

const emit = defineEmits(['mode-change']);

const setActiveMode = (mode) => {
  activeMode.value = mode;
  emit('mode-change', mode);
};

onMounted(() => {
  // Emit initial mode
  emit('mode-change', activeMode.value);
});
</script>

<style lang="scss" scoped>
.chat-voice-toggle {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  .active-button {
    border-radius: 100px;
    background: #f4ecff;
    display: flex;
    padding: 4px 16px;
    justify-content: center;
    align-items: center;
    gap: 10px;
    cursor: pointer;
    transition: background-color 0.2s ease;

    &:hover {
      background: #e8d9ff;
    }

    .text {
      color: #431a80;
      text-align: center;
      font-size: 16px;
      font-weight: 500;
    }
  }
  .inactive-button {
    display: flex;
    padding: 4px 16px;
    justify-content: center;
    align-items: center;
    gap: 10px;
    cursor: pointer;
    border-radius: 100px;
    transition: background-color 0.2s ease;

    &:hover {
      background: rgba(244, 236, 255, 0.5);
    }

    .text {
      color: var(--gray-400, #9ca3af);
      text-align: center;
      font-size: 16px;
      font-weight: 500;
    }
  }
}
</style>
