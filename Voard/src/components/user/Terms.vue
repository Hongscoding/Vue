!
<template>
  <v-app>
    <v-app-bar>
      <v-app-bar-title>약관</v-app-bar-title>
    </v-app-bar>
    <v-main>
      <v-container clas>
        <v-sheet max-width="800" class="mx-auto mt-16">
          <v-textarea
            label="이용약관"
            variant="outlined"
            rows="10"
            hide-details="true"
            v-model="state.data.terms"
          ></v-textarea>
          <v-checkbox
            class="d-flex justify-end"
            label="동의"
            v-model="state.isCheck1"
          ></v-checkbox>
          <v-textarea
            label="개인정보 취급방침"
            variant="outlined"
            rows="10"
            hide-details="true"
            v-model="state.data.privacy"
          ></v-textarea>
          <v-checkbox
            class="d-flex justify-end"
            label="동의"
            v-model="state.isCheck2"
          ></v-checkbox>
        </v-sheet>
        <v-sheet max-width="800" class="mx-auto text-center">
          <v-btn @click="btnCancel">취소</v-btn>
          <v-btn @click="btnNext" color="primary" class="ml-2">다음</v-btn>
        </v-sheet>
      </v-container>
    </v-main>
    <v-footer app theme="dark">copyright &copy; Voard v1.8</v-footer>
  </v-app>
</template>
<script setup>
import axios from "axios";
import { onBeforeMount } from "vue";
import { reactive } from "vue";
import { useRouter } from "vue-router";

const router = useRouter();

const state = reactive({
  data: {},
  isCheck1: false,
  isCheck2: false,
});

const btnCancel = () => {
  router.push("/user/login");
};
const btnNext = () => {
  if (state.isCheck1 && state.isCheck2) {
    router.push("/user/register");
  } else {
    alert("동의 체크");
  }
};

onBeforeMount(() => {
  axios
    .get("http://localhost:8080/Voard/user/terms")
    .then((response) => {
      console.log(response);
      state.data = response.data;
    })
    .catch((error) => {
      console.log(error);
    });
});
</script>
<style scoped></style>
