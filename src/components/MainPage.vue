<template>
  <div class="container">
    <h1 class="title text-5xl font-medium">Hoşgeldin {{ cookies.username }}</h1>
    <p class="subtext-main text-slate-500">
      Lütfen telefonu açma sebebinizi belirtiniz
    </p>
    <v-form ref="form">
      <v-combobox
        v-model="model"
        class="combobox"
        variant="outlined"
        label="Kullanım Sebepleri"
        density="compact"
        :rules="required"
        :items="baseReasons"
        return-object
        :disabled="is_saved"
      ></v-combobox>

      <div v-if="!is_saved" class="saveButton">
        <v-btn @click="save" color="primary">Kaydet</v-btn>
      </div>
      <div v-else>Cevabınız için teşekkür ederiz.</div>
    </v-form>

    <p class="subtext-bottom text-slate-500">Daha önceki cevaplarınız</p>
    <div v-for="reason in baseReasons" class="flex">
      <p>{{ reason.title }}</p>
      <p>{{ reason.count }}</p>
    </div>
  </div>
</template>

<script setup lang="ts">
import { onMounted, ref } from "vue";
import bg from "../assets/bg.jpg";
const props = defineProps(["cookies"]);

const model = ref();
const required = [(v) => !!v || "Bu alan boş bırakılamaz."];
const emit = defineEmits(["refresh"]);
const form = ref();
const is_saved = ref(false);
const baseReasons = ref([
  {
    title: "Sosyal Medya",
    count: 12,
  },
  {
    title: "Müzik Dinlemek",
    count: 4,
  },
  {
    title: "Oyun oynamak",
    count: 2,
  },
]);

async function save() {
  const resp = await form.value.validate();
  if (resp.valid) {
    if (model.value?.count) {
      model.value.count = model.value?.count + 1;
    } else {
      baseReasons.value.push({
        title: model.value,
        count: 1,
      });
    }
    is_saved.value = true;
    console.log(model.value);
  }
}

console.log(props.cookies);
</script>

<style>
.container {
  background: #cadce4d7;
  border-radius: 12px;
  padding: 12px;
  height: 100%;
  width: 100%;
}
.combobox {
  padding-top: 32px;
}
.subtext-main {
  font-size: 14px;
  color: #475569;
}
.subtext-bottom {
  padding-left: 12px;
  padding-right: 12px;
  font-size: 14px;
  color: #475569;
  padding-top: 24px;
}
.flex {
  padding-left: 12px;
  font-size: 14px;
  padding-top: 4px;
  padding-right: 12px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>
