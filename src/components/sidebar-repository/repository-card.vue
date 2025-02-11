<template>
  <div class="flex items-center justify-between font-bold text-[#948aec]">
    <h2 class="hover:underline">
      <a :href="repository.html_url" rel="noopener noreferrer">
        <span>{{ repository.owner.login }} / {{ repository.name }}</span>
      </a>
    </h2>

    <a
      v-if="repository.homepage"
      :href="repository.homepage"
      rel="noopener noreferrer"
    >
      <svg-icon name="link" class="ml-2" />
    </a>
  </div>

  <ul class="flex flex-wrap text-xs text-gray-300" @click="handleClickTopic">
    <li
      v-for="topic in repository.topics"
      :key="topic"
      :data-topic="topic"
      :class="{
        'selected-tag':
          tagStore.selectedType === TAG_TYPE.topic &&
          tagStore.selected === topic,
      }"
      class="mr-1 mt-1 rounded-full border border-solid border-gray-300 px-2 hover:border-[#948aec] hover:bg-[#948aec] hover:!text-white"
    >
      {{ topic }}
    </li>
  </ul>

  <div class="my-3 text-xs text-[#666]">{{ repository.description }}</div>

  <div class="flex justify-between text-xs font-bold text-[#76d0a3]">
    <div>
      <span class="mr-3 inline-flex items-center">
        <svg-icon name="star-fill" class="mr-1" />
        <span>{{ repository.stargazers_count }}</span>
      </span>
      <span class="inline-flex items-center">
        <svg-icon name="branch" class="mr-1" />
        <span>{{ repository.forks_count }}</span>
      </span>
    </div>
    <span>{{ repository.language }}</span>
  </div>
</template>

<script setup>
import { useTagStore } from '@/store/tag';
import { TAG_TYPE } from '@/constants';

defineProps({
  repository: {
    type: Object,
    required: true,
  },
});

const tagStore = useTagStore();

function handleClickTopic(e) {
  let elTag = e.target;
  while (!elTag.dataset.topic) {
    elTag = elTag.parentElement;
    if (!elTag) return;
  }
  tagStore.$patch({
    selectedType: TAG_TYPE.topic,
    selectedNav: TAG_TYPE.topic,
    selected: elTag.dataset.topic,
  });
}
</script>

<style scoped>
.selected-tag {
  border-color: var(--primary);
  color: var(--primary);
}
</style>
