<template>
  <div 
    class="prompt-input-wrapper"
    @dragover="handleDragOver"
    @dragleave="handleDragLeave"
    @drop="handleDrop"
    @dragenter="handleDragEnter"
  >
    <!-- Drop overlay -->
    <div 
      v-show="isDragOver" 
      class="drop-overlay"
      :class="{ visible: isDragOver }"
      @dragover.prevent
      @drop="handleDrop"
    >
      <div class="drop-content">
        <span class="drop-text">Dropped files appear here</span>
      </div>
    </div>
    
    <div class="prompt-input-container">
      <div class="dropped-file-wrapper" v-if="droppedFileUrl">
        <div class='dropped-file'>
          <img :src="droppedFileUrl" alt="Dropped file preview" class="dropped-image" />
        </div>
        <div  class="remove-file-btn" @click="removeDroppedFile">
          <img :src="appImages['X.svg']" alt="Remove" class="remove-file-icon"/>
        </div>
      </div>
      <div class="input-section">
        <textarea
          v-model="inputValue"
          class="prompt-textarea"
          :placeholder="placeholder"
          @keydown.enter="handleSubmit"
          @input="checkAndResize"
          ref="textareaRef"
        ></textarea>
      </div>
      <div class="attach-catalog-submit-container">
        <div class="attach-catalog-container">
          <slot name="attachCatalog">
            <div class="attach-icon-wrapper">
              <slot name="attachIcon">
                <svg width="17" height="16" viewBox="0 0 17 16" fill="none" xmlns="http://www.w3.org/2000/svg">
                  <path
                    d="M7.81348 15.1667C5.33348 15.1667 3.31348 13.1467 3.31348 10.6667V6.66667C3.31348 3.82 5.63348 1.5 8.48014 1.5C11.3268 1.5 13.6468 3.82 13.6468 6.66667V10.3333C13.6468 11.8933 12.3735 13.1667 10.8135 13.1667C9.25348 13.1667 7.98014 11.8933 7.98014 10.3333V8C7.98014 7.72667 8.20681 7.5 8.48014 7.5C8.75348 7.5 8.98014 7.72667 8.98014 8V10.3333C8.98014 11.3467 9.80014 12.1667 10.8135 12.1667C11.8268 12.1667 12.6468 11.3467 12.6468 10.3333V6.66667C12.6468 4.36667 10.7801 2.5 8.48014 2.5C6.18014 2.5 4.31348 4.36667 4.31348 6.66667V10.6667C4.31348 12.5933 5.88014 14.1667 7.81348 14.1667C8.08681 14.1667 8.31348 14.3933 8.31348 14.6667C8.31348 14.94 8.09348 15.1667 7.81348 15.1667Z"
                    fill="#4B5563"
                  />
                </svg>
              </slot>
            </div>
            <div class="catalog-wrapper">
              <slot name="catalogIcon">
                <svg width="17" height="16" viewBox="0 0 17 16" fill="none" xmlns="http://www.w3.org/2000/svg">
                  <path
                    d="M8.90409 1.47203C8.66409 1.28536 8.33078 1.28536 8.09078 1.47203C6.82411 2.4387 3.08408 5.59203 3.10408 9.26537C3.10408 12.2387 5.52411 14.6654 8.50411 14.6654C11.4841 14.6654 13.9041 12.2454 13.9041 9.27204C13.9108 5.65203 10.1641 2.44536 8.90409 1.47203Z"
                    stroke="#4B5563"
                    stroke-miterlimit="10"
                  />
                  <path d="M8.5 1.33203V14.6654" stroke="#4B5563" stroke-linecap="round" stroke-linejoin="round" />
                  <path
                    d="M8.5 12.6398L13.6333 10.1465"
                    stroke="#4B5563"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                  />
                  <path
                    d="M8.5 9.30659L13.4133 6.91992"
                    stroke="#4B5563"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                  />
                  <path
                    d="M8.5 5.97318L11.8534 4.33984"
                    stroke="#4B5563"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                  />
                </svg>
                <div class="catalog-text">Choose from catalog</div>
              </slot>
            </div>
          </slot>
        </div>
        <div class="submit-button-container" @click="handleSubmit">
          <slot name="submitButton">
            <svg
              v-if="!isLoading"
              xmlns="http://www.w3.org/2000/svg"
              width="16"
              height="16"
              viewBox="0 0 16 16"
              fill="none"
            >
              <path
                d="M12.0467 6.88094C11.9201 6.88094 11.7934 6.83427 11.6934 6.73427L8.00008 3.04094L4.30674 6.73427C4.11341 6.9276 3.79341 6.9276 3.60008 6.73427C3.40674 6.54094 3.40674 6.22094 3.60008 6.0276L7.64675 1.98094C7.84008 1.7876 8.16008 1.7876 8.35341 1.98094L12.4001 6.0276C12.5934 6.22094 12.5934 6.54094 12.4001 6.73427C12.3067 6.83427 12.1734 6.88094 12.0467 6.88094Z"
                fill="#4B5563"
              />
              <path
                d="M8 14.1673C7.72667 14.1673 7.5 13.9406 7.5 13.6673V2.44727C7.5 2.17393 7.72667 1.94727 8 1.94727C8.27333 1.94727 8.5 2.17393 8.5 2.44727V13.6673C8.5 13.9406 8.27333 14.1673 8 14.1673Z"
                fill="#4B5563"
              />
            </svg>
            <svg v-else width="33" height="32" viewBox="0 0 33 32" fill="none" xmlns="http://www.w3.org/2000/svg">
              <circle cx="16.5" cy="16" r="16" fill="#F3F4F6" />
              <rect x="9" y="8" width="16" height="16" rx="2" fill="#4B5563" />
            </svg>
          </slot>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, defineProps, defineEmits, inject } from 'vue';

const appImages = inject('appImages');

const props = defineProps({
  placeholder: {
    type: String,
    default: 'Type here what task do you want me to do or upload image',
  },
  isLoading: {
    type: Boolean,
    default: false,
  },
  acceptedFileTypes: {
    type: Array,
    default: () => ['*/*'], // Accept all file types by default
  },
  maxFileSize: {
    type: Number,
    default: 100 * 1024 * 1024, // 100MB
  },
});

const emit = defineEmits(['submit', 'filesDropped']);

const inputValue = ref('');
const textareaRef = ref(null);
const isDragOver = ref(false);
const droppedFile = ref(null);
const droppedFileUrl = ref('');

const handleSubmit = () => {
  if (inputValue.value.trim()) {
    emit('submit', inputValue.value.trim());
    // Don't clear inputValue here - let parent handle it after API call
  }
};

// Drag and drop handlers
const handleDragOver = (e) => {
  e.preventDefault();
  e.stopPropagation();
  isDragOver.value = true;
};

const handleDragEnter = (e) => {
  e.preventDefault();
  e.stopPropagation();
  isDragOver.value = true;
};

const handleDragLeave = (e) => {
  e.preventDefault();
  e.stopPropagation();
  // Only set to false if we're leaving the entire drop zone
  if (!e.currentTarget.contains(e.relatedTarget)) {
    isDragOver.value = false;
  }
};

const handleDrop = (e) => {
  e.preventDefault();
  e.stopPropagation();
  isDragOver.value = false;

  const files = Array.from(e.dataTransfer.files);
  
  if (files.length > 0) {
    const validFiles = files.filter(file => {
      // Check file type
      const isValidType = props.acceptedFileTypes.some(type => {
        if (type === 'image/*') {
          return file.type.startsWith('image/');
        }
        if (type === '*/*') {
          return true;
        }
        return file.type === type;
      });

      // Check file size
      const isValidSize = file.size <= props.maxFileSize;

      return isValidType && isValidSize;
    });

    if (validFiles.length > 0) {
      emit('filesDropped', validFiles);
      droppedFile.value = validFiles[0];
      if (droppedFile.value && droppedFile.value.type.startsWith('image/')) {
        droppedFileUrl.value = URL.createObjectURL(droppedFile.value);
      } else {
        droppedFileUrl.value = '';
      }
    }
  }
};

// Method to clear input - can be called from parent via ref
const clearInput = () => {
  inputValue.value = '';
  autoResize();
};

// Auto-resize textarea
const autoResize = () => {
  if (textareaRef.value) {
    // Reset to auto to get the natural height
    textareaRef.value.style.height = 'auto';
    
    // Get the scroll height and set the new height
    const scrollHeight = textareaRef.value.scrollHeight;
    const newHeight = Math.max(40, Math.min(scrollHeight, 200));
    textareaRef.value.style.height = newHeight + 'px';
  }
};

const checkAndResize = () => {
  if (textareaRef.value) {
    // Reset to auto to get the natural height needed
    textareaRef.value.style.height = 'auto';
    const scrollHeight = textareaRef.value.scrollHeight;

    // Set the appropriate height with a more generous threshold
    if (scrollHeight <= 60) {
      // Content fits in single line (increased threshold to account for padding/line-height)
      textareaRef.value.style.height = '40px';
    } else {
      // Content needs multiple lines
      textareaRef.value.style.height = Math.min(scrollHeight, 200) + 'px';
    }
  }
};

const removeDroppedFile = () => {
  droppedFile.value = null;
  droppedFileUrl.value = '';
};

// Expose methods to parent component
defineExpose({
  clearInput,
});
</script>

<style lang="scss" scoped>
.prompt-input-wrapper {
  display: flex;
  flex-direction: column;
  //   align-items: flex-start;
  gap: 0.5rem;
  //   align-self: stretch;
  justify-content: center;
  max-width: 50.8125rem;
  width: 100%;
  position: relative;

  .drop-overlay {
    display: flex;
    padding: 16px;
    flex-direction: column;
    justify-content: space-between;
    align-items: flex-start;
    border-radius: 8px;
    border: 1px solid #431A80;
    background: rgba(249, 250, 251, 0.15);
    box-shadow: 0px 0px 32px 0px rgba(67, 26, 128, 0.25);
    backdrop-filter: blur(50px);
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    z-index: 2;
    transition: opacity 0.7s cubic-bezier(0.4,0,0.2,1), visibility 0.7s cubic-bezier(0.4,0,0.2,1);
    opacity: 0;
    pointer-events: none;
    visibility: hidden;

    &.visible {
      opacity: 1;
      pointer-events: auto;
      visibility: visible;
    }

    .drop-content {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      width: 100%;
      height: 100%;
      text-align: center;

      .drop-text {
        color: var(--gray-700, #374151);
        text-align: center;
        font-family: 'Nunito', sans-serif;
        font-size: 20px;
        font-style: normal;
        font-weight: 600;
        line-height: normal;
        letter-spacing: 0;
        margin: 0;
        padding: 0 20px;
      }
    }
  }

  .prompt-input-container {
    border-radius: 0.5rem;
    border: 1px solid var(--gray-100, #f3f4f6);
    background: rgba(249, 250, 251, 0.75);
    backdrop-filter: blur(50px);
    display: flex;
    max-width: 50.8125rem;
    min-height: 7.75rem;
    padding: 1rem;
    flex-direction: column;
    justify-content: space-between;
    align-items: flex-start;
    gap: 1rem;
    transition: border-color 0.2s ease;

    &:hover {
      border-color: #d1d5db;
    }

    .attach-catalog-container {
      display: flex;
      align-items: center;
      gap: 0.25rem;
    }

    .input-section {
      display: flex;
      width: 100%;
      gap: 0.75rem;
      align-items: flex-end;

      .prompt-textarea {
        flex: 1;
        min-height: 40px;
        height: 40px;
        max-height: 200px;
        border: none;
        border-radius: 0.5rem;
        font-size: 0.875rem;
        resize: none;
        outline: none;
        transition: border-color 0.2s ease;
        background: transparent;
        overflow-y: hidden;
        font-size: 1rem;
        font-weight: 500;
        color: var(--gray-600, #4b5563);

        &:focus {
          outline: none;
          box-shadow: none;
        }

        &::placeholder {
          color: var(--gray-400, #9ca3af);
        }
      }

      .submit-button {
        padding: 0.75rem 1.5rem;
        background-color: #3b82f6;
        color: white;
        border: none;
        border-radius: 0.5rem;
        font-size: 0.875rem;
        font-weight: 500;
        cursor: pointer;
        transition: all 0.2s ease;
        white-space: nowrap;

        &:hover:not(:disabled) {
          background-color: #2563eb;
        }

        &:disabled {
          background-color: #d1d5db;
          cursor: not-allowed;
        }
      }
    }
  }

  .attach-catalog-submit-container {
    display: flex;
    justify-content: space-between;
    align-items: center;
    align-self: stretch;
    .attach-catalog-container {
      display: flex;
      //   width: 390px;
      align-items: center;
      gap: 0.25rem;
      .attach-icon-wrapper {
        display: flex;
        cursor: pointer;
      }
      .catalog-wrapper {
        display: flex;
        // height: 32px;
        padding: 0.25rem 0.5rem;
        justify-content: center;
        align-items: center;
        gap: 0.25rem;
        border-radius: 0.5rem;
        background: var(--white, #fff);
        cursor: pointer;
        .catalog-text {
          color: var(--gray-600, #4b5563);
          text-align: center;
          font-size: 1rem;
          font-weight: 500;
        }
      }
    }
    .submit-button-container {
      width: 2rem;
      height: 2rem;
      flex-shrink: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      flex-shrink: 0;
      background: #f3f4f6;
      border-radius: 50%;
      transition: all 0.3s ease;
      cursor: pointer;
      &:hover {
        background: #e5e7eb;
        svg {
          path {
            fill: #431a80;
          }
        }
      }
    }
  }
}

.dropped-file-wrapper {
  position: relative;
  display: inline-block;
  margin-bottom: 8px;
  &:hover .remove-file-btn {
    opacity: 1;
    pointer-events: auto;
  }
}
.dropped-file {
  width: 60px;
  height: 60px;
  aspect-ratio: 1/1;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
}
.dropped-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 8px;
}
.remove-file-btn {
  position: absolute;
  top: -10px;
  right: -10px;
  width: 24px;
  height: 24px;
  border: none;
  background: rgba(255,255,255,0.95);
  color: #431A80;
  border-radius: 50%;
  cursor: pointer;
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.2s;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 2;
  box-shadow: 0 2px 8px rgba(67,26,128,0.10);
  .remove-file-icon{
    width: 1rem;
    height: 1rem;
  }
}
</style>
