<template>
  <v-main>
     <v-container class="mb-10">
      <v-card class="pa-10" color="rgba(255, 255, 255, 0.5)">
    <v-row class="ma-auto" justify="center">
      <v-col cols="4">
        <v-text-field
          v-model="member.email"
          label="E-mail"
           prepend-inner-icon="mdi-email"
          solo
          readonly
        ></v-text-field>

        <v-text-field
          v-model="member.name"
          label="이름"
          prepend-inner-icon="mdi-account-circle"
              solo
          clearable
        ></v-text-field>

        <v-text-field
          v-model="member.phone"
          label="휴대폰"
          prepend-inner-icon="mdi-phone"
              solo
          clearable
        ></v-text-field>

        <v-textarea
          v-model="member.profile"
          label="소개"
          prepend-inner-icon="mdi-message-reply-text"
              solo
          clearable
        ></v-textarea>
      </v-col>
    </v-row>
    </v-card>
     </v-container>

    <v-row class="ma-auto" justify="center">
      <v-col cols="auto">
        <v-btn color="primary" class="mx-6" @click="modify()"
          >수정완료</v-btn
        >
        <v-btn class="mx-6" @click="detail()">돌아가기</v-btn>
      </v-col>
    </v-row>
      
  </v-main>
</template>

<script>
import axios from "../../axios/axios-common";
export default {
  created(){
    //회원정보를 받아옴
    //백엔드 연동 후 아래 주석 해제
    this.getmember();
  },
  methods: {
   getmember() {//회원정보 조회
      axios
        .get("/member/" + this.$route.params.memberno)
        .then((response) => 
        { if(response.data.status)
          this.member = response.data.object;
          
        })
        .catch((exp) => alert(exp + "조회에 실패하였습니다."));
    },
    modify(){//회원정보 수정
      axios
      .put('/member/'+this.$route.params.memberno,{
        name : this.member.name,
        phone : this.member.phone,
        profile : this.member.profile
        })
      .then((response)=>{
        if(response.data.status){//성공하면 알림 후 마이페이지로 이동
          alert("수정성공!");
          this.$router.push("/member/" + this.$route.params.memberno);
        }else{//실패하면 알림 후 수정페이지 다시 불러옴
          alert("수정실패!");
          this.getmember();
        }
      })
      .catch((exp)=>alert(exp+"수정에 실패하였습니다."));
    },
    detail(){
      //마이페이지로 이동
        this.$router.push("/member/" + this.$route.params.memberno);
    }
  },
  data() {
    return {
      member: {}
    };
  },
};
</script>

<style>

</style>