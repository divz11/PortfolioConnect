<template>
  <div class="auth">
    <div class="auth-box">
      <img src="@/assets/brand/BrandLogo.png" alt="" class="logo" />
      <login-page class="page" v-if="LoginRegister == false">
        <button @click="LoginRegisterToggle">
          SIGN UP &nbsp;<span>&#8594;</span>
        </button>
      </login-page>
      <register-page class="page" v-else>
        <button @click="LoginRegisterToggle">GO BACK</button>
      </register-page>
    </div>
  </div>
</template>

<script>
import LoginPage from "./Login.vue";
import RegisterPage from "./Register.vue";

export default {
  components: {
    LoginPage,
    RegisterPage,
  },
  methods: {
    LoginRegisterToggle() {
      this.$store.commit("LoginRegisterToggle");
    },
  },
  computed: {
    LoginRegister() {
      return this.$store.state.LoginRegister;
    },
  },
};
</script>

<style lang="scss">
@media screen and (min-width: 1080px) {
  .auth {
    height: 100vh;
    width: 100vw;
    z-index: 20;
    position: fixed;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    align-items: center;
    &-box {
      min-height: 35vw;
      min-width: 45vw;
      margin: auto;
      background-color: rgba(0, 0, 0, 0.7);
      overflow: hidden;
      .logo {
        position: relative;
        width: 300px;
      }
      .page {
        display: flex;
        flex-flow: column nowrap;
        align-items: center;
        position: relative;
        bottom: 50px;
        input,
        select {
          width: 250px;
          height: 30px;
          margin: 10px;
          background-color: rgba(20, 1, 86, 1);
          border: none;
          color: #e3507d;
          padding: 5px;
          text-align: center;
          font-weight: 600;
          appearance: none;
          &:focus {
            border: none;
          }
        }
        select {
          color: rgba(227, 80, 125, 0.5);
          width: 260px;
          height: 40px;
        }
        button {
          cursor: pointer;
          width: 260px;
          height: 40px;
          margin: 10px;
          background-color: #140156;
          border: 2px solid #e3507d;
          color: #e3507d;
          font-weight: 600;
          span {
            color: #e3507d;
            font-weight: 900;
          }
          &:hover {
            background-color: #e3507d;
            background-image: linear-gradient(
              to right,
              #e29867,
              #ec8469,
              #f26e75,
              #f05a89,
              #e34da4,
              #d842b3,
              #c73cc3,
              #ae3dd5,
              #a330dd,
              #9422e6,
              #8114ef,
              #6607f9
            );
            color: whitesmoke;
            span {
              color: whitesmoke;
            }
          }
          &:active:enabled {
            transform: scale(0.95);
          }
        }
        .text {
          margin-top: 10px;
          font-weight: 500;
        }
        .error {
          color: #ff000dc5;
          margin: 10px;
          text-decoration: underline;
        }
      }
    }
  }
}
</style>
