<template>
  <v-app>
    <v-app-bar>
      <v-app-bar-title>로그인</v-app-bar-title>
    </v-app-bar>
    <v-main>
      <v-container>
        <v-card class="mx-auto mt-16" max-width="400">
          <v-card-item>
            <v-card-title>로그인</v-card-title>
          </v-card-item>
          <v-card-text>
            <v-container>
              <v-row>
                <v-col cols="8">
                  <v-text-field
                    label="아이디"
                    prepend-icon="mdi-account"
                    variant="underlined"
                    hide-details="true"
                    v-model="user.uid"
                  ></v-text-field>
                  <v-text-field
                    type="password"
                    label="비밀번호"
                    prepend-icon="mdi-lock"
                    variant="underlined"
                    hide-details="true"
                    v-model="user.pass"
                  ></v-text-field>
                </v-col>
                <v-col cols="4">
                  <v-btn @click="btnLogin" block height="90" color="primary"
                    >로그인</v-btn
                  >
                </v-col>
              </v-row>
            </v-container>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn @click="btnRegister">회원가입</v-btn>
          </v-card-actions>
        </v-card>
      </v-container>
    </v-main>
    <v-footer app theme="dark">copyright &copy; Voard v1.8</v-footer>
  </v-app>
</template>
<script setup>
import axios from "axios";
import { reactive } from "vue";
import { useRouter } from "vue-router";
import { useStore } from "vuex";

const router = useRouter();
const userStore = useStore();

const user = reactive({
  uid: null,
  pass: null,
});

const btnLogin = () => {
  axios
    .post("http://localhost:8080/Voard/user/login", user)
    .then((response) => {
      console.log(response);

      const accessToken = response.data.accessToken;
      const user = response.data.user;

      localStorage.setItem("accessToken", accessToken); // 로컬스토리지에 토큰 저장
      userStore.dispatch("setUser", user); //스토어에 user 객체 정보 저장

      router.push("/list");
    })
    .catch((error) => {
      console.log(error);
    });
};
const btnRegister = () => {
  router.push("/user/terms");
};
</script>
<style scoped></style>
