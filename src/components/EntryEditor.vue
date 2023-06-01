<script lang="ts" setup>
import EmojiField from "@/components/EmojiField.vue";
import ArrowCircleRight from "@/assets/icons/arrow-circle-right.svg";
import type Emoji from "@/types/Emoji";
import { ref, computed } from "vue";
import type Entry from "@/types/Entry";

const text = ref("");
const emoji = ref<Emoji | null>(null);
const charCount = computed(() => text.value.length);
const maxChars = 280;

const handleTextInput = (e: Event) => {
  const textarea = e.target as HTMLTextAreaElement;
  if (textarea.value.length <= maxChars) {
    text.value = textarea.value;
  } else {
    text.value = textarea.value = textarea.value.substring(0, maxChars);
  }
};

const emit = defineEmits<{
  (e: "@create", entry: Entry): void;
}>();

const handleSubmit = () => {
  emit('@create', {
      body: text.value,
      emoji: emoji.value,
      createdAt: new Date(),
      userId: 1,
      id: Math.random()
  });
  text.value = "";
  emoji.value = null
}


</script>
<template>
  <form
    class="entry-form"
    @submit.prevent="handleSubmit">


    <textarea
      :value="text"
      @keyup="handleTextInput"
      placeholder="New Journal Entry for danielkelly_io"
    ></textarea>
    <EmojiField v-model="emoji" />
    <div class="entry-form-footer">
      <span>{{ charCount }} / {{ maxChars }}</span>
      <button>Remember <ArrowCircleRight width="20" /></button>
    </div>
  </form>
</template>