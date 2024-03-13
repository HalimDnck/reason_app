<template>
  <div class="w-full h-full px-6 py-12 base">
    <LoginPage v-if="!is_login" @refresh="checkUserName"></LoginPage>
    <MainPage v-else :cookies="definedCookies"></MainPage>
  </div>
</template>

<script setup lang="ts">
import { onMounted, ref } from "vue";
import LoginPage from "./components/LoginPage.vue";
import MainPage from "./components/MainPage.vue";
const is_login = ref(false);
const definedCookies = ref();

function checkUserName() {
  const cookies = document?.cookie?.split(";").reduce((cookies, cookie) => {
    const [name, value] = cookie.trim().split("=");
    cookies[name] = value;
    return cookies;
  }, {});
  definedCookies.value = cookies;

  if (cookies && definedCookies.value.username) {
    is_login.value = true;
  } else {
    is_login.value = false;
  }
}

onMounted(() => {
  checkUserName();
});
</script>
<style>
.base {
  height: 100vh;
  width: 100vw;
  background-image: url("./assets/bg.jpg");
  background-repeat: no-repeat;
  background-attachment: fixed;
  background-position: center;
  display: flex;
  align-items: start;
  justify-content: center;
}
</style>
