<template>
  <v-container>
    <v-card v-if="!me">
      <v-form ref="form" @submit.prevent="onSubmitForm" v-model="valid">
        <v-container>
          <v-text-field
            v-model="userId"
            label="아이디"
            :rules="userIdRules"
            required
          />
          <v-text-field
            v-model="password"
            label="비밀번호"
            type="password"
            :rules="passwordRules"
            required
          />
          <v-btn
            class="white--text"
            color="green"
            type="submit"
            :disabled="!valid"
          >
            로그인
          </v-btn>
          <v-btn
            nuxt
            to="/signup"
            type="submit"
          >
            회원가입
          </v-btn>
        </v-container>
      </v-form>
    </v-card>
    <v-card v-else>
      <v-container>
        {{me.nickname}}
        <v-btn @click="onLogOut">로그아웃</v-btn>
        <v-row>
          <v-col cols="4">{{me.Followings.length}} 팔로잉</v-col>
          <v-col cols="4">{{me.Followers.length}} 팔로워</v-col>
          <v-col cols="4">{{me.Posts.length}} 게시글</v-col>
        </v-row>
      </v-container>
    </v-card>
  </v-container>
</template>

<script>
  export default {
    data() {
      return {
        valid: false,
        userId: '',
        userIdRules: [
          v => !!v || '아이디는 필수입니다.',
        ],
        password: '',
        passwordRules: [
          v => !!v || '비밀번호는 필수입니다.',
        ],
      };
    },
    computed: {
      me() {
        return this.$store.state.users.me;
      },
    },
    methods: {
      onSubmitForm() {
        if (this.$refs.form.validate()) {
          this.$store.dispatch('users/logIn', {
            userId: this.userId,
            password: this.password,
          });
        }
      },
      onLogOut() {
        this.$store.dispatch('users/logOut');
      },
    },
  };
</script>
