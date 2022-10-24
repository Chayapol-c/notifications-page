<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import Card from './components/Card.vue';
import information from './assets/information.json';
import TimeAgo from 'javascript-time-ago'
import en from 'javascript-time-ago/locale/en'

import { computed, ref } from 'vue';

TimeAgo.addDefaultLocale(en)
// Create formatter (English).
const timeAgo = new TimeAgo('en-US')

const notificationNumber = ref(0);
const notifications = ref([]);

notifications.value = [...information];
notificationNumber.value = information.reduce((total, ele) => ele.isRead ? total : total+1, 0);

const onMarkAllRead = () => {
  notificationNumber.value = 0;
  notifications.value = notifications.value.map( ele => ({...ele, isRead: true}));
};

const getNotificationTime = (time) => {
  return timeAgo.format(Date.now() - time);
};

</script>

<template>
  <main>
    <header>
      <div class="title">
        Notifications
        <div
          class="notification-number" 
          v-if="notificationNumber > 0">
          {{ notificationNumber }}
        </div>
      </div>
      <button class="btn-mark-all" @click.stop="onMarkAllRead">
        Mark all as read
      </button>
    </header>
    <div class="notification-container">
      <template
        v-for="profile in notifications"
        :key="profile.id">
        <Card
          :title="profile.title"
          :text="profile.activityText"
          :profileImageUrl="profile.profileImageUrl"
          :isRead="profile.isRead"
          :notificationTime="getNotificationTime(profile.age)"
        >
          <div v-if="profile?.privateMessage" class="private-message">
            {{profile.privateMessage}}
          </div>
          <template v-slot:highlight-text>
            <span
              v-if="profile?.postName"
              class="post-name">
              {{profile.postName}}
            </span>
            <span
              v-if="profile?.groupName"
              class="group-name">
              {{profile.groupName}}
            </span>
          </template>
          <template
            v-slot:commentPicture
            v-if="profile?.commentedPicture">
            <img class="comment-image" :src="profile.commentedPicture" alt="">
          </template>
        </Card>
      </template>
    </div>
  </main>
</template>

<style lang="scss" scoped>
@import "./assets/scss/variables.scss";
main {
  background-color: $white;
  border-radius: 1rem;
  margin: 3rem auto;
  max-width: 750px;
  padding: 1.5rem 2rem;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 2rem;
}
.title {
  color: $blue-900;
  font-weight: $font-weight-bold;
  font-size: 1.5rem;
  display: inline-flex;
  align-items: center;
}
.notification-number {
  color: $white;
  background-color: $primary;
  padding: 0.1rem 0.6rem;
  border-radius: 5px;
  margin-left: 0.5rem;
  font-size: 1rem;
}
.notification-container {
  > div {
    margin-bottom: 0.5rem;
  }
}
.btn-mark-all {
  color: hsl(219, 12%, 42%);
  background-color: transparent;
  border: 0;
  font-size: 1rem;
  cursor: pointer;

  &:hover {
    text-decoration: underline;
  }
}

@media screen and (max-width: $mobile-breakpoint) {
  body {
    background-color: $white;
  }
  main {
    padding: 1.5rem 1rem;
    border-radius: 0;
    min-height: 100vh;
  }
}
</style>
