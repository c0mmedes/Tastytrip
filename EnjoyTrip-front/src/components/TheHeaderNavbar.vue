<template>
  <div>
    <!-- <b-navbar toggleable="lg" class="my-custom-navbar" type="dark" variant="dark"> -->
    <b-navbar toggleable="lg" class="my-custom-navbar">
      <b-navbar-brand href="#">
        <router-link :to="{ name: 'main' }">
          <b-img
            :src="require('@/assets/ssafy_logo.png')"
            id="logo"
            class="d-inline-block align-top"
            alt="logo"
          ></b-img>
        </router-link>
      </b-navbar-brand>

      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

      <b-collapse id="nav-collapse" is-nav>
        <b-navbar-nav>
          <b-nav-item id="item" href="#">
            <router-link :to="{ name: 'main' }" class="m-2 link"> 메인 </router-link>
            <router-link :to="{ name: 'attr' }" class="m-2 link"> 여행지 </router-link>
            <router-link :to="{ name: 'board' }" class="m-2 link"> 게시판 </router-link>

          </b-nav-item>
        </b-navbar-nav>

        <!-- after login -->
        <b-navbar-nav class="ml-auto" v-if="userInfo">
          <b-nav-item class="align-self-center">
            <b-avatar variant="primary" v-text="userInfo.userid.charAt(0).toUpperCase()"></b-avatar>
            {{ userInfo.username }}({{ userInfo.userid }})님 환영합니다.
          </b-nav-item>
          <b-nav-item class="align-self-center">
            <b-dropdown id="dropdown-right" right text="마이페이지" variant="primary" class="m-2">
              <b-dropdown-item>
                <router-link :to="{ name: 'mypage' }" class="link align-self-center">
                  회원정보
                </router-link>
              </b-dropdown-item>
              <b-dropdown-item>
                <router-link :to="{ name: 'myplan' }" class="link align-self-center">
                  내 여행지
                  <b-badge variant="primary" pill>14</b-badge>
                </router-link>
              </b-dropdown-item>
            </b-dropdown>
          </b-nav-item>
          <b-nav-item class="align-self-center link" @click.prevent="onClickLogout">
            로그아웃
          </b-nav-item>
        </b-navbar-nav>
        <!-- before login -->
        <b-navbar-nav class="ml-auto" v-else>
          <b-nav-item-dropdown right>
            <template #button-content>
              <b-icon icon="people" font-scale="2"></b-icon>
            </template>
            <b-dropdown-item href="#">
              <router-link :to="{ name: 'join' }" class="link">
                회원가입
              </router-link>
            </b-dropdown-item>
            <b-dropdown-item href="#">
              <router-link :to="{ name: 'login' }" class="link">
                로그인
              </router-link>
            </b-dropdown-item>
          </b-nav-item-dropdown>
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>
  </div>
</template>

<script>
import { mapState, mapGetters, mapActions } from "vuex";

const memberStore = "memberStore";

export default {
  name: "TheHeaderNavbar",
  data() {
    return {};
  },
  computed: {
    ...mapState(memberStore, ["isLogin", "userInfo", "isDeleteSuccess"]),
    ...mapGetters(["checkUserInfo"]),
  },
  methods: {
    ...mapActions(memberStore, ["userLogout"]),
    // ...mapMutations(memberStore, ["SET_IS_LOGIN", "SET_USER_INFO"]),
    onClickLogout() {
      // this.SET_IS_LOGIN(false);
      // this.SET_USER_INFO(null);
      // sessionStorage.removeItem("access-token");
      // if (this.$route.path != "/") this.$router.push({ name: "main" });
      console.log(this.userInfo.userid);
      //vuex actions에서 userLogout 실행(Backend에 저장 된 리프레시 토큰 없애기
      //+ satate에 isLogin, userInfo 정보 변경)
      // this.$store.dispatch("userLogout", this.userInfo.userid);
      this.userLogout(this.userInfo.userid);
      sessionStorage.removeItem("access-token"); //저장된 토큰 없애기
      sessionStorage.removeItem("refresh-token"); //저장된 토큰 없애기
      if (this.$route.path != "/") this.$router.push({ name: "main" });
    },
  },
};
</script>

<style scoped>
#logo {
  width: 120px;
}

.link {
  text-decoration: none;
}
</style>
