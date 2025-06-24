<template>
  <div class="ai-home-page-wrapper">
    <div class="animated-background"></div>
    <div class="header">
      <img :src="appImages['asian-paints-logo.png']" class="asian-paints-logo" />
      <!-- <img :src="appImages['more-actions.svg']"/> -->
    </div>
    <div class="ai-home-page-content-wrapper">
      <!-- <div></div> -->
      <div class="asian-paints-ai-heading-wrapper">
        <img :src="appImages['ai-logo.svg']" class="ai-logo-wrapper heartbeat-animation" />
        <div class="asian-paints-ai-text">Asian Paints AI</div>
      </div>
      <div class="chat-voice-prompt-wrapper">
        <ChatVoiceToggle
          :buttons="[
            { id: 'chat', text: 'Chat' },
            { id: 'voice', text: 'Voice' },
          ]"
          @mode-change="handleModeChange"
        />
        <PromptInput ref="promptInputRef" :is-loading="isProcessing" @submit="handlePromptSubmit" />
        <div class="generate-an-image-wrapper">
          <img :src="appImages['image-icon.svg']" />
          <div class="generate-an-image-text">Generate an Image</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref, inject } from 'vue';
import { useRoute } from 'vue-router';
import PromptInput from './PromptInput.vue';
import ChatVoiceToggle from './ChatVoiceToggle.vue';

const appImages = inject('appImages');

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
  position: relative;
  overflow: hidden;

  .animated-background {
    position: absolute;
    bottom: -180px;
    left: 50%;
    transform: translateX(-50%);
    width: 600px;
    height: 150px;
    border-radius: 175px;
    background: #ff7316;
    filter: blur(100px);
    animation: morph 8s ease-in-out infinite;
    z-index: 1;
  }
  .header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    align-self: stretch;
    .asian-paints-logo {
      width: 7.625rem;
      height: 1.5rem;
      aspect-ratio: 61/12;
    }
  }
  .ai-home-page-content-wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
    // align-self: stretch;
    max-width: 50.8125rem;
    flex-grow: 1;
    justify-content: center;
    gap: 4rem;
    width: 100%;
    .asian-paints-ai-heading-wrapper {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 1rem;
      .asian-paints-ai-text {
        align-self: stretch;
        color: var(--gray-600, #4b5563);
        text-align: center;
        font-size: 1.5rem;
        font-weight: 700;
      }
      .ai-logo-wrapper {
        border-radius: 50%;
      }
    }
    .chat-voice-prompt-wrapper {
      display: flex;
      flex-direction: column;
      align-items: flex-start;
      gap: 0.5rem;
      align-self: stretch;
      width: 100%;
    }
    .generate-an-image-wrapper {
      display: flex;
      padding: 0.5rem 1rem;
      align-items: center;
      gap: 0.5rem;
      border-radius: 0.5rem;
      border: 1px solid var(--gray-100, #f3f4f6);
      cursor: pointer;
      transition: all 0.3s ease-in;
      margin-top: 1.5rem;
      align-self: center;
      .generate-an-image-text {
        color: var(--gray-500, #6b7280);
        text-align: center;
        font-size: 1rem;
        font-weight: 500;
        transition: all 0.3s ease-in;
      }
      &:hover {
        background-color: #f8f9fa;
        border-color: #d1d5db;
        .generate-an-image-text {
          color: #374151;
        }
      }
    }
  }
}

@keyframes morph {
  0%,
  100% {
    border-radius: 175px;
    height: 150px;
  }
  25% {
    border-radius: 100px;
    height: 200px;
  }
  50% {
    border-radius: 50px;
    height: 180px;
  }
  75% {
    border-radius: 75px;
    height: 220px;
  }
}
</style>
