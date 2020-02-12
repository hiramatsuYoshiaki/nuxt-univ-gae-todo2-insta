<template>
  <div class="content">
    <div v-if="isWaiting">
      <div class="loading-wrape">
        <p>login.....</p>
        <svg
          class="spinner"
          width="65px"
          height="65px"
          viewBox="0 0 66 66"
          xmlns="http://www.w3.org/2000/svg"
        >
          <circle
            class="path"
            fill="none"
            stroke-width="6"
            stroke-linecap="round"
            cx="33"
            cy="33"
            r="30"
          />
        </svg>
      </div>
    </div>
    <div v-else>
      <div class="content-wrap">
        <div class="top-image">
          <div class="auth-title">
            <h2>INSTA TODOS</h2>
            <h6>私も撮りたい！</h6>
            <h6>インスタグラムで</h6>
            <h6>見た素敵な写真を．．．</h6>
          </div>
          <div class="top-image1">
            <div class="cache-image">
              <img
                class="img-ipad"
                src="~assets/img/instaIpad.png"
                alt="insta iPad"
              />
              <!-- </div>
            <div class="cache-image"> -->
              <img
                class="img-iphone"
                src="~assets/img/insta_iPhone8.png"
                alt="insta iPhne8"
              />
            </div>
          </div>
          <div class="auth-guid">
            フォトアクティビティが楽しくなる。Todosリストに、お気に入りのInstagramを表示できる。
          </div>
        </div>
        <div class="auth">
          <div class="auth-title">
            <h6>サインインすれば簡単に使える。</h6>
            <h4>まずは、使ってみよう。</h4>
          </div>
          <div v-if="isAuthenticated">
            <h6>こんにちは、</h6>
            <h4 class="auth-user">
              {{ user.email }}
            </h4>
            <h6>さん</h6>
            <p>{{ user.email }}さんでログイン中です。</p>
            <!-- <p>e-mail:{{ user.email }}</p> -->
            <div class="add-btn">
              <button @click="logout">
                ログアウト
              </button>
            </div>
            <!-- <a href="/works">CRTU</a> -->
          </div>
          <div v-else>
            <div class="auth-guid">
              <h6>Demoでログインしてみる。</h6>
              <p>デモユーザーでログインする場合は、以下を入力してください。</p>
              <p>メール：demo@gmail.com</p>
              <p>パスワード：demo1111</p>
            </div>
            <div class="login-form">
              <form novalidate @submit.prevent="loginCheck">
                <div v-if="authErrors.length">
                  <p class="error-title">
                    入力項目を確認してください。
                  </p>
                  <ul>
                    <li v-for="(error1, index) in authErrors" :key="index">
                      <p class="error-msg">
                        {{ error1 }}
                      </p>
                    </li>
                  </ul>
                </div>
                <p>
                  <input
                    v-model="email"
                    type="text"
                    placeholder="メール"
                    required
                    :style="{ background: error.emailBg }"
                  />
                </p>
                <p>
                  <input
                    v-model="password"
                    type="password"
                    placeholder="パスワード"
                    required
                    :style="{ background: error.passwordBg }"
                  />
                </p>
                <p v-if="register">
                  <input
                    v-model="displayName"
                    type="text"
                    placeholder="ユーザー"
                    required
                    :style="{ background: error.displayNameBg }"
                  />
                </p>

                <p>
                  <input id="checkbox" v-model="register" type="checkbox" />
                  <label for="checkbox">新規登録</label>
                </p>
                <div class="add-btn">
                  <button type="submit">
                    {{ register ? '新規登録' : 'ログイン' }}
                  </button>
                </div>
              </form>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import { mapActions, mapState, mapGetters } from 'vuex'
import { ADD_REGISTORY, GET_REGISTORY } from '~/store/actionTypes'
import firebase from '@/plugins/firebase'
export default {
  //   props: {
  //     pageTitle: {
  //       type: String,
  //       default: 'a'
  //     },
  //   }
  data() {
    return {
      errors: [],
      email: null,
      password: null,
      displayName: null,
      isWaiting: false,
      register: null,
      error: {
        emailBg: '#e3f2fd',
        passwordBg: '#e3f2fd',
        displayNameBg: '#e3f2fd'
      },
      img_iphone: '~assets/img/insta_iPhone8.png',
      img_ipad: '~assets/img/instaIpad.png'
    }
  },
  computed: {
    ...mapState(['user']),
    ...mapState(['regstar']),
    ...mapState(['authErrors']),
    ...mapGetters(['isAuthenticated'])
  },
  mounted() {
    this.$store.commit('clearAuthError')
    firebase.auth().onAuthStateChanged((user) => {
      if (user) {
        // console.log('uid: ' + user.uid)
        // console.log('email: ' + user.email)
        // console.log('displayName: ' + user.displayName)
        const loginUser = {
          uid: user.uid,
          email: user.email,
          displayName: ''
        }
        this.$store.commit('setUser', loginUser)
        // this.setUser(loginUser)
        this.$store.dispatch(GET_REGISTORY, loginUser)

        // console.log('not setTimeout: ' + this.user) // ここだと取得できない
        // setTimeout(() => {
        // console.log('setTimeout: ' + this.user.email) // ここだと取得できる
        // なにかしらの処理
        // })
      }
    })
  },
  methods: {
    ...mapActions(['setUser']),
    loginCheck(e) {
      this.$store.commit('clearAuthError')
      this.error.emailBg = '#e3f2fd'
      this.error.passwordBg = '#e3f2fd'
      this.error.displayNameBg = '#e3f2fd'
      // alert('loginCheck')
      // this.$store.commit('clearAuthError')

      // this.$store.commit('setAuthError', 'passowrd required.')
      // this.$store.commit('setAuthError', 'Email required.')

      // alert(this.isAuthError)

      // this.errors = []
      if (!this.password) {
        this.$store.commit('setAuthError', 'パスワードは必須です。')
        this.error.passwordBg = '#f8bbd0'
      } else if (this.password.length < 8) {
        this.$store.commit('setAuthError', 'パスワードは８文字以上です。')
        this.error.passwordBg = '#f8bbd0'
      }
      if (!this.email) {
        this.$store.commit('setAuthError', 'メールは必須です。')
        this.error.emailBg = '#f8bbd0'
      } else if (!this.validEmail(this.email)) {
        this.$store.commit('setAuthError', '無効なメール形式です。')
        this.error.emailBg = '#f8bbd0'
      }
      // if (email.length < 4) {
      //     alert('Please enter an email address.');
      //     return;
      //   }
      if (this.register) {
        alert('reg')
        if (!this.displayName) {
          alert('reg error')
          this.$store.commit('setAuthError', 'ユーザー名は必須です。')
          this.error.displayNameBg = '#f8bbd0'
        } else if (this.displayName.length > 31) {
          this.$store.commit('setAuthError', 'ユーザー名は３０文字以下です。')
          this.error.displayNameBg = '#f8bbd0'
        }
      }
      // console.log('error' + this.errors)
      // if (this.errors.length) this.login()
      // this.login()

      if (this.authErrors.length) {
        // alert('error')
      } else {
        // alert('normal')
        this.login()
      }
      e.preventDefault()
    },
    login() {
      console.log('login')
      // if (!this.formIsValidSignin) {
      //   return
      // }
      // if (!this.formIsValidLogin) {
      //   return
      // }
      // if (!this.image) {
      //   return
      // }
      this.isWaiting = true
      if (this.register) {
        console.log('signin')
        firebase
          .auth()
          .createUserWithEmailAndPassword(this.email, this.password)
          .then((res) => {
            // console.log('createUserWithEmailAndPassword')
            const user = firebase.auth().currentUser
            // console.log('uid: ' + user.uid)
            // console.log('email: ' + user.email)
            // console.log('displayName: ' + this.displayName)
            return user
          })
          .then((user) => {
            // console.log('firebase auth add user')
            // console.log('uid: ' + user.uid)
            // console.log('email: ' + user.email)
            // console.log('displayName: ' + this.displayName)
            this.$store.dispatch(ADD_REGISTORY, {
              uid: user.uid,
              email: user.email,
              displayName: this.displayName
            })
          })
          .then((user) => {
            this.isWaiting = false
            const lp = '/crud'
            this.link_commit(lp)
          })
          .catch((error) => {
            // alert('signin error' + error)
            // console.log('signin error' + error)
            this.isWaiting = false
            this.$store.commit('setAuthError', error)
          })
      } else {
        // console.log('login email pass')
        firebase
          .auth()
          .signInWithEmailAndPassword(this.email, this.password)
          .then((user) => {
            this.isWaiting = false
            const lp = '/crud'
            // setTimeout(() => {
            this.link_commit(lp)
            // }, 1000)
          })
          .catch((error) => {
            // alert('login error' + error)
            console.log('login error' + error)
            this.isWaiting = false
            // this.errors.push('Invalid email .')
            this.$store.commit('setAuthError', error)
          })
      }
    },
    validEmail: (email) => {
      /* eslint-disable */
      const re = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
      /* eslint-disable */
      return re.test(email)
    },
    logout() {
      console.log('logout')
      firebase
        .auth()
        .signOut()
        .then(() => {
          this.$store.commit('setUser', null)
        })
        .catch((error) => {
          console.log('logout error' + error)
        })
    },
    link_commit(linkPath) {
      // this.active = true
      this.$store.commit('pagePathSet', linkPath)
      setTimeout(() => {
        this.$router.push({ path: linkPath }) // non-leload
      }, 500)
    }
  }
}
</script>
<style scoped lang="scss">
$offset: 187;
$duration: 1.4s;
.content {
  position: relative;
  width: 100%;
  height: 100%;
  padding: 2rem 0.5rem;
  @media (min-width: 768px) {
    padding: 2rem 4rem;
  }
}

.loading-wrape {
  width: 100%;
  height: 100%;
  margin: 3rem;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  @media (min-width: 992px) {
    margin: 5rem;
  }
}
.content-wrap {
  position: relative;
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: flex-start;
  @media (min-width: 992px) {
    flex-direction: row;
    justify-content: flex-start;
    align-items: flex-start;
  }
}

.top-image,
.auth {
  width: 100%;
  height: 100%;
  padding: 2rem;
  @media (min-width: 992px) {
    width: 50%;
    height: 100%;
    padding: 4rem 4rem;
    flex-direction: row;
    justify-content: flex-start;
    align-items: flex-start;
  }
  @media (min-width: 1440px) {
    width: 50%;
    height: 100%;
    padding: 4rem 8rem;
    flex-direction: row;
    justify-content: flex-start;
    align-items: flex-start;
  }
}
.cache-image {
  width: 100%;
  display: flex;
  flex-direction: column-reverse;
  justify-content: center;
  align-items: center;
  @media (min-width: 992px) {
    flex-direction: row;
  }
}
.img-ipad {
  width: 18rem;
  height: auto;
  margin: 1rem;
  @media (min-width: 992px) {
    width: 12rem;
  }
  @media (min-width: 1440px) {
    width: 18rem;
  }
}

.img-iphone {
  width: 10rem;
  height: auto;
  margin: 1rem;
  @media (min-width: 992px) {
    width: 8rem;
  }
  @media (min-width: 1440px) {
    width: 10rem;
  }
}
.auth-title {
  margin-bottom: 2rem;
  h4 {
    font-size: 1.6rem;
    font-weight: 600;
    @media (min-width: 992px) {
      font-size: 2.4rem;
    }
  }
  h6 {
    font-size: 1.2rem;
    @media (min-width: 992px) {
      font-size: 1.6rem;
    }
  }
}
.auth-user {
  color: dodgerblue;
}
.auth-guid {
  width: 100%;
  margin-top: 2rem;
  word-wrap: break-word;
  overflow-wrap: break-word;
}
.add-btn button {
  border: none;
  background-color: $footer-color-color;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.19), 0 6px 6px rgba(0, 0, 0, 0.23);
  color: #fff;
  margin-top: 1em;
  outline: 0;
}
.login-form {
  width: 100%;
  height: 100%;
  // overflow: scroll;
  padding: 1rem;
  @media (min-width: 992px) {
    border: 1px solid gray;
    padding: 2rem;
  }
}

.error-title {
  font-weight: 600;
  font-size: 1rem;
  line-height: 1rem;
  margin-bottom: 1rem;
  color: rgb(2, 2, 2);
  @media (min-width: 992px) {
    font-weight: 600;
    font-size: 1rem;
    line-height: 1rem;
  }
}
.error-msg {
  font-weight: 600;
  font-size: 1rem;
  line-height: 1rem;
  margin-bottom: 1rem;
  color: rgb(190, 29, 29);
  margin-left: 1rem;
  @media (min-width: 992px) {
    font-weight: 600;
    font-size: 1rem;
    line-height: 1rem;
  }
}

.spinner {
  animation: rotator $duration linear infinite;
}

@keyframes rotator {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(270deg);
  }
}

.path {
  stroke-dasharray: $offset;
  stroke-dashoffset: 0;
  transform-origin: center;
  animation: dash $duration ease-in-out infinite,
    colors ($duration * 4) ease-in-out infinite;
}

@keyframes colors {
  0% {
    stroke: #4285f4;
  }
  25% {
    stroke: #de3e35;
  }
  50% {
    stroke: #f7c223;
  }
  75% {
    stroke: #1b9a59;
  }
  100% {
    stroke: #4285f4;
  }
}

@keyframes dash {
  0% {
    stroke-dashoffset: $offset;
  }
  50% {
    stroke-dashoffset: $offset/4;
    transform: rotate(135deg);
  }
  100% {
    stroke-dashoffset: $offset;
    transform: rotate(450deg);
  }
}
</style>
