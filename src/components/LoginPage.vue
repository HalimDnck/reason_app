<template>
  <div class="container">
    <h1 class="title text-5xl font-medium">Hoşgeldiniz</h1>
    <p class="subtext text-slate-500">Lütfen kullanıcı adınızı giriniz</p>
    <v-form ref="form" class="w-full">
      <v-text-field
        class="username"
        density="compact"
        variant="outlined"
        v-model="username"
        :rules="required"
        label="Kullanıcı Adı"
      ></v-text-field>
      <div class="saveButton">
        <v-btn @click="saveusername" color="primary">Kaydet</v-btn>
      </div>
    </v-form>
    <!-- <p class="errortext"></p> -->
  </div>
</template>

<script setup lang="ts">
import { onMounted, ref } from "vue";

const username = ref("");
const emit = defineEmits(["refresh"]);
const form = ref();
const required = [(v) => !!v || "Bu alan boş bırakılamaz."];

async function saveusername() {
  const resp = await form.value.validate();
  if (resp.valid) {
    document.cookie = `username=${username.value}`;
    emit("refresh");
  }
}
</script>

<style>
.title {
  font-size: 24px;
}
.saveButton {
  padding-top: 12px;
  display: flex;
  justify-content: flex-end;
  width: 100%;
}
.username {
  padding-top: 32px;
  padding-bottom: 32px;
}
</style>
