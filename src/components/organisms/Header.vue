<script lang="ts" setup>
import { ref } from "vue";
import Button from "@/components/molecules/Button.vue";
import Sidebar from "primevue/sidebar";
import SearchModal from "@/modules/trips/components/organisms/SearchModal.vue";
import UserButton from "@/components/molecules/UserButton.vue";
import { useUserStore } from "@/stores/user";
import { useAuthStore } from "@/stores/auth";

const userStore = useUserStore();
const authStore = useAuthStore();
userStore.setUser();

const displaySidebar = ref<boolean>(false);
const modalIsOpen = ref<boolean>(false);
</script>

<template>
  <div
    class="sticky z-50 top-0 bg-white w-full h-20 py-4 px-5 shadow-md flex justify-between"
  >
    <RouterLink :to="{ name: 'home' }" class="w-52">
      <img
        class="w-full h-full"
        src="@/assets/images/logos/logo_full.svg"
        alt="logo"
      />
    </RouterLink>
    <div class="hidden md:flex">
      <template v-if="userStore.isAuth">
        <Button
          class="mx-2"
          icon="fa-search"
          icon-size="2xl"
          @click="modalIsOpen = true"
        />
        <Button
          class="mx-2"
          icon="fa-plus"
          :text="$t('header.newTrip')"
          icon-size="2xl"
        />
        <!--<Button class="mx-2" icon="fa-regular fa-bell" icon-size="2xl" />-->
        <UserButton />
      </template>
      <template v-else>
        <Button
          :to="{ name: 'login' }"
          class="mx-2"
          icon="fa-lock"
          :text="$t('auth.login')"
          icon-size="2xl"
        />
        <Button
          :to="{ name: 'register' }"
          class="mx-2"
          icon="fa-user-plus"
          :text="$t('auth.register')"
          icon-size="2xl"
        />
      </template>
    </div>
    <div class="md:hidden">
      <Button
        class="mx-2"
        icon="fa-bars"
        icon-size="2xl"
        @click="displaySidebar = !displaySidebar"
      />
    </div>
  </div>
  <Sidebar v-model:visible="displaySidebar" position="right">
    <template #header>
      <div class="w-32">
        <img class="w-full" src="@/assets/images/logos/logo_full.svg" alt="" />
      </div>
    </template>
    <div>
      <template v-if="userStore.isAuth">
        <Button
          class="mx-2 my-4"
          icon="fa-search"
          :text="$t('header.search')"
          icon-size="2xl"
          @click="
            () => {
              displaySidebar = false;
              modalIsOpen = true;
            }
          "
        />
        <Button
          class="mx-2 my-4"
          icon="fa-plus"
          :text="$t('header.newTrip')"
          icon-size="2xl"
        />
        <!--<Button
        class="mx-2 my-4"
        icon="fa-regular fa-bell"
        :text="$t('header.notifications')"
        icon-size="2xl"
      />-->
        <Button
          :to="{ name: '' }"
          class="mx-2 my-4"
          icon="fa-user"
          :text="$t('header.profile')"
          icon-size="2xl"
        />
        <Button
          class="mx-2 my-4"
          icon="fa-sign-out-alt"
          :text="$t('auth.logout')"
          icon-size="2xl"
          @click="
            () => {
              authStore.logout();
              displaySidebar = false;
            }
          "
        />
      </template>
      <template v-else>
        <Button
          @click="displaySidebar = false"
          :to="{ name: 'login' }"
          class="mx-2 mt-10 mb-4"
          icon="fa-lock"
          :text="$t('auth.login')"
          icon-size="2xl"
        />
        <Button
          @click="displaySidebar = false"
          :to="{ name: 'register' }"
          class="mx-2 my-4"
          icon="fa-regular fa-pen-to-square"
          :text="$t('auth.register')"
          icon-size="2xl"
        />
      </template>
    </div>
  </Sidebar>
  <SearchModal :isOpen="modalIsOpen" @update:isOpen="modalIsOpen = $event" />
</template>

<style lang="scss">
.p-sidebar-header {
  justify-content: space-between !important;
}
</style>
