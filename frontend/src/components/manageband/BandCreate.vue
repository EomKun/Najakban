<template>
  <v-main>
     <v-container class="mb-10">
      <v-card class="pa-10" color="rgba(255, 255, 255, 0.5)">
    <v-row justify="center">
      <v-col cols="6">
        <v-banner class="my-10">
          <strong>밴드개설</strong>
        </v-banner>
      </v-col>
    </v-row>
    <v-row class="px-10" justify="center">
      <v-col cols="6">
        <v-text-field
          v-model="band.name"
          :counter="20"
          label="밴드명"
          prepend-inner-icon="mdi-account-circle"
              solo
          clearable
          :rules="[(v) => v.length <= 20 || '밴드명이 너무 깁니다']"
          required
        >
        </v-text-field>

      </v-col>
    </v-row>
    <v-row class="px-10" justify="center">
      <v-col cols="3">
        <v-select
          :items="genre"
          label="밴드장르"
          v-model="band.genre"
          required
          solo
        ></v-select>
      </v-col>
      <v-col cols="3">
        <v-select
          :items="color"
          v-model="band.color"
          label="밴드색깔"
          required
          solo

        ></v-select>
      </v-col>
    </v-row>
    <v-row class="px-10" justify="center">
      <v-col cols="6">
        <v-select
          solo
          label="밴드장 세션"
          :items="sessions"
          v-model="band.codeSession"
          
        ></v-select>
      </v-col>
    </v-row>
    <v-row class="px-10" justify="center">
      <v-col cols="6">
        <v-textarea
          v-model="band.intro"
          label="밴드소개"
          prepend-inner-icon="mdi-message-reply-text"
              solo
          clearable
          required
          :counter="200"
          :rules="[(v) => v.length <= 200 || '밴드소개가 너무 깁니다']"
        ></v-textarea>
      </v-col>
    </v-row>
      </v-card>
     </v-container>


    <v-row class="ma-auto">
      <v-col cols="auto" class="ma-auto">
        <v-btn color="primary" class="mx-6" @click="create()">밴드개설</v-btn>

        <v-btn  class="mx-6" @click="bandlist()">돌아가기</v-btn>
      </v-col>
    </v-row>
  </v-main>
</template>

<script>
import { mapGetters,mapActions } from "vuex"; //vuex사용
const MemberStore = "MemberStore"; //MemberStore 모듈 사용
const BandStore = "BandStore"; //MemberStore 모듈 사용

export default {
  data: () => {
    return {
      image: null,
      band: {
        name: "",
        img: "",
        intro: "",
        color: "",
        genre: "",
        codeSession : '',
      },
      sessions: ["보컬", "키보드", "드럼", "기타", "베이스"],
      color: [
        "하양",
        "빨강",
        "주황",
        "노랑",
        "초록",
        "파랑",
        "검정",
      ],
      genre: ["팝", "락", "재즈"],
    };
  },
  computed: {
    ...mapGetters(MemberStore, ["getMemberId"]),
  },
  methods: {
    ...mapActions(BandStore,["reqCreateBand"]),
    create() {
      //공백이 존재하면 경고
      if (this.band.name == "" || this.band.intro == "") {
        alert("공백이 존재합니다!");
        return;
      }
      if(this.band.color == "" || this.band.genre == ""){
        alert("장르 및 컬러를 선택해주세요 ");
        return;
      }
      if (this.band.codeSession == ''){
        alert("세션을 선택해주세요!");
        return;
      }
      this.reqCreateBand({memberId : this.getMemberId,
          codeSession : this.band.codeSession,
          name: this.band.name,
          intro: this.band.intro,
          genre: this.band.genre,
          color: this.band.color,})
        .then((response) => {
          if (response.result) {
            //성공 시 알려주고 push
            alert(response.msg + "! 밴드 사진과 세션을 등록해주세요");
            this.$router.push("/band/list/" + this.getMemberId);
          } else {
            alert(response.msg);
          }
        })
        .catch((exp) => alert(exp + "밴드를 생성할 수 없습니다."));
    },
    bandlist() {
      //밴드리스트 페이지로 이동
      this.$router.push("/band/list/" + this.getMemberId);
    },
  },
};
</script>

<style>
</style>