<template>
    <form v-if="formStore.displayQuestions?.length" class="flex flex-col gap-y-[1rem]" @submit.prevent="console.log('submit')">
        <component v-for="(question, index) in formStore.displayQuestions" :is="mapTypeComponents[question.type]" :data="question"
            :index="index" :is-visible="isVisible(index)">
        </component>
    </form>
    <a id="downloadLink" style="display: none">Download</a>
</template>

<script setup>
import { useFormStore } from '../stores/store'
import BooleanSelector from './BooleanSelector.vue';
import TextInput from './TextInput.vue'

const props = defineProps({
    questions: {
        type: Array,
        required: true,
    }
})

const mapTypeComponents = {
    // boolean: BooleanSelector,
    text: TextInput,
};

const isVisible = (index) => {
    if (index === 0) return true
    return formStore.questions[index - 1]?.value !== undefined;
}

// Store
const formStore = useFormStore();
</script>
