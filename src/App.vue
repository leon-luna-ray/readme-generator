<template>
  <div id="vue-app" :class="['py-[1rem]', globalStore.theme]">
    <div class="container">
      <header class="flex justify-between items-center">
        <h1>{{ content.title }}</h1>
        <ThemeControls />
      </header>
      <main class="main flex-col-2">
        <div class="intro flex-col-2">
          <TerminalText :string="content.intro" :speed="500" />
          <div v-if="isReady" class="flex gap-x-[1rem]">
            <p><span v-if="!formStore.isFormStarted" class="animate-pulse">{{ globalStore.indicator }}</span> {{
              content.ready_text }}</p>
            <BooleanSelector @true="formStore.setIsFormStarted" @false="reloadPage" />
          </div>
        </div>
        <Form v-if="formStore.isFormStarted" :questions="content.questions" />

        <div v-if="formStore.isFormReady" class="flex gap-x-[1rem]" ref="downloadRef">
          <p><span class="animate-pulse">{{ globalStore.indicator }}</span> {{ content.download_text }}</p>
          <BooleanSelector @true="formStore.handleSubmit" @false="reloadPage" />
        </div>
      </main>
    </div>
  </div>
</template>
<script setup>
import { ref, onMounted, watch } from 'vue';
import { storeToRefs } from 'pinia';
import { useGlobalStore, useFormStore } from './stores/store'
import { content } from './lib/content'

import BooleanSelector from './components/BooleanSelector.vue'
import Form from './components/Form.vue'
import ThemeControls from './components/ThemeControls.vue'
import TerminalText from './components/TerminalText.vue'

// Store
const globalStore = useGlobalStore();
const formStore = useFormStore();

// State
const { isFormReady } = storeToRefs(formStore);
const isReady = ref(false);
const downloadRef = ref(null);

// Methods
const reloadPage = () => {
  location.reload()
}
const scrollToBottom = () => {
  setTimeout(() => {
    console.log('should scroll');
    if (downloadRef.value) {
      downloadRef.value.scrollIntoView({ behavior: 'smooth' });
    }
  }, 100);
};


// Watchers
watch(isFormReady, () => {
  console.log('form is ready')
  scrollToBottom();
})

// Lifecycle
onMounted(() => {
  formStore.setQuestions(content.questions);

  setTimeout(() => {
    isReady.value = true;
  }, 2000)
})
</script>
