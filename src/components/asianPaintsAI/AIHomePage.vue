<template>
  <div class="ai-home-page-wrapper">
    <div class="ai-home-page-content-wrapper">
      <div class="chat-voice-prompt-wrapper">
        <ChatVoiceToggle 
          :buttons="[
            { id: 'chat', text: 'Chat' },
            { id: 'voice', text: 'Voice' }
          ]"
          @mode-change="handleModeChange" 
        />
        <PromptInput ref="promptInputRef" :is-loading="isProcessing" @submit="handlePromptSubmit" />
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue';
import { useRoute } from 'vue-router';
import PromptInput from './PromptInput.vue';
import ChatVoiceToggle from './ChatVoiceToggle.vue';

const route = useRoute();
const isProcessing = ref(false);
const promptInputRef = ref(null);
const currentMode = ref('chat'); // Track current mode

const handleModeChange = (mode) => {
  currentMode.value = mode;
  console.log('Mode changed to:', mode);

  if (mode === 'voice') {
    handleVoiceInput();
  } else {
    handleChatMode();
  }
};

const handlePromptSubmit = async (prompt) => {
  if (isProcessing.value) return;

  isProcessing.value = true;
  try {
    console.log('Submitting prompt:', prompt);
    console.log('Current mode:', currentMode.value);
    // TODO: Implement actual API call to AI service
    // const response = await submitPromptToAI(prompt);

    // Simulate API call
    await new Promise((resolve) => setTimeout(resolve, 2000));
    console.log('Prompt submitted successfully');

    // Clear input after successful API call
    promptInputRef.value?.clearInput();
  } catch (error) {
    console.error('Error submitting prompt:', error);
  } finally {
    isProcessing.value = false;
  }
};

const handleVoiceInput = () => {
  console.log('Voice input mode activated');
  // TODO: Implement voice input functionality
  // This could open a voice recording interface or change UI behavior
};

const handleChatMode = () => {
  console.log('Chat mode activated');
  // TODO: Implement chat mode functionality
  // This could switch to a different UI mode
};

onMounted(() => {
  console.log('AI Home Page mounted');
});
</script>

<style lang="scss" scoped>
.ai-home-page-wrapper {
  display: flex;
  padding: 1rem;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 1rem;
  .ai-home-page-content-wrapper{
    display: flex;
    flex-direction: column;
    align-items: center;
    // align-self: stretch;
    max-width: 50.8125rem;
    .chat-voice-prompt-wrapper {
      display: flex;
      flex-direction: column;
      align-items: flex-start;
      gap: 0.5rem;
      align-self: stretch;
    }
  }
}
</style>
