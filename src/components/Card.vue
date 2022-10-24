<script setup>
import { computed } from "@vue/runtime-core";

defineProps({
  title: {
    type: String,
    default: '',
    required: true,
  },
  text: {
    type: String,
    default: '',
    required: true,
  },
  profileImageUrl: {
    type: String,
    default: '',
    required: true,
  },
  isRead: {
    type: Boolean,
    default: false,
    required: true,
  },
  notificationTime: {
    type: String,
  }
});

</script>
<template>
  <div class="card-container" :class="{'is-read': !isRead}">
    <img class="card-profile" :src="profileImageUrl" :alt="`${title}'s profile image`">
    <div class="card-content">
      <p class="card-text">
        <span
          class="card-title"
          v-if="title">
          {{ title }}
        </span>
        <span class="card-text">
          {{ text }}
          <slot name="highlight-text" />
          <span v-if="!isRead" class="unread-dot"></span>
        </span>
      </p>
      <p class="time-text">
        {{ notificationTime }}
      </p>
      <slot />
    </div>
    <div v-if="$slots['commentPicture']">
      <slot name="commentPicture" />
    </div>
  </div>
</template>
<style lang="scss" scoped>
@import "../assets/scss/variables.scss";

.card-container {
  display: flex;
  column-gap: 1rem;
  padding: 1rem 1rem;
  border-radius: 0.5rem;
  
  &.is-read {
    background-color: $blue-100;
  }
  .card-content {
    flex: 1;
  }
  .card-profile {
    width: 2.5rem;
    height: 2.5rem;
  }

  .card-title {
    font-weight: 800;
    color: $blue-900;
    margin-right: 0.5rem;
  }

  .card-text {
    color: $blue-500;
  }
  .time-text {
    margin-top: 0.25rem;
    color: $blue-400;
  }

  .unread-dot {
    width: 0.5rem;
    height: 0.5rem;
    background-color: $red;
    border-radius: 9999px;
    display: inline-block;
    margin-bottom: 0.1rem;
  }
  
  :slotted(.private-message) {
    border: 1px solid $blue-200;
    color: $blue-500;
    border-radius: 5px;
    padding: 1rem;
    margin-top: 0.5rem;
  }
  :slotted(.comment-image) {
    width: 2.5rem;
    height: 2.5rem;
  }
  :slotted(.post-name) {
    font-weight: 800;
    margin: 0 0.25rem
  }
  :slotted(.group-name) {
    color: $primary;
    font-weight: 800;
    margin: 0 0.25rem
  }
}
</style>