<template>
  <v-main>
    <v-container class="mb-10">
      <v-card class="pa-10" color="rgba(255, 255, 255, 0.5)">
      <v-layout col-8 offset-2 column>
        <!-- title -->
        <v-flex text-center class="ma-10">
          <h1>공지사항 수정</h1>
        </v-flex>

        <!-- icons - modify, delete -->
        <v-flex text-right>
          <v-btn
          class="mx-7"
            icon
            color="blue"
            large
            @click="onModify()"
          >
            <v-icon size="50">mdi-check</v-icon>
          </v-btn>

          <v-btn
            icon
            color="primary"
            large
            @click="
              () => {
                this.$router.go(-1);
              }
            "
          >
            <v-icon size="40">mdi-undo-variant</v-icon>
          </v-btn>
        </v-flex>

        <!-- content -->
        <v-flex>
          <v-layout>
            <v-flex col-2 text-right><h2>제목</h2></v-flex>
            <v-flex col-1><v-spacer /></v-flex>
            <v-flex col-7>
              <v-text-field solo v-model="title">
            </v-text-field>
            </v-flex>
          </v-layout>
          <v-layout>
            <v-flex col-2 text-right><h2>내용</h2></v-flex>
            <v-flex col-1><v-spacer /></v-flex>
            <v-flex col-7>
              <v-textarea
              solo
              height="200"
              style="margin: 20px auto"
              v-model="content"
            ></v-textarea>
            </v-flex>
          </v-layout>
        </v-flex>
      </v-layout>
      </v-card>
    </v-container>
  </v-main>
</template>

<script>
import { mapActions, mapGetters } from 'vuex';

const NoticeStore = "NoticeStore";
const MemberStore = "MemberStore";

export default {
  data(){
    return {
      title: "",
      content: ""
    };
  },
  created(){
    if(this.getManager === false)  this.$router.go("/*");
    console.log()
    this.title = this.getNotice.noticeTitle;
    this.content = this.getNotice.noticeContent;
  },
  computed: {
    ...mapGetters(NoticeStore, ["getNotice"]),
    ...mapGetters(MemberStore, ["getManager"]),
  },
  methods: {
    ...mapActions(NoticeStore, ["reqModifyNotice"]),
    //공지사항 내용 수정
    onModify() {
      this.reqModifyNotice({ no: this.$route.params.noticeno, title: this.title, content: this.content })
      .then((response) => {
        if(response.result){  
          alert(response.msg);
          this.$router.push("/notice/"+this.$route.params.noticeno)
          } 
        else                  alert(response.msg);
      })
      .catch((error) => {
        console.log(error);
      })
    },
  },
};
</script>

<style></style>
