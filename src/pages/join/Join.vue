<template>
  <q-page class="q-pa-md page-default">
    <div class="row justify-center">
      <div class="col-12 doc-heading doc-h2">
        {{ $t('menu_join') }}
      </div>
    </div>
    <q-separator />
    <div class="row justify-center q-pt-sm q-pb-sm">
      <div class="col-12">
        {{ $t('menu_join_description') }}
      </div>
    </div>

    <!-- wallet_address -->
    <div class="row justify-center q-pt-md">
      <div class="col-12">
        <span class="text-weight-bold text-subtitle1">{{ $t('user_wallet_address') }}</span>
      </div>
    </div>
    <div class="row justify-center">
      <div class="col-12">
        <q-input v-model="getWalletAddress" ref="wallet_address" :rules="[required, val => minLength(val, 6), val => maxLength(val, 50)]" standout tabindex="1" />
      </div>
    </div>

    <!-- email(uid) -->
    <div class="row justify-center q-pt-md">
      <div class="col-12">
        <span class="text-weight-bold text-subtitle1">{{ $t('email') }}</span>
      </div>
    </div>
    <div class="row">
      <div class="col-6 q-pr-sm">
        <q-input v-model="uid" ref="uid" :rules="[required, val => minLength(val, 6), val => maxLength(val, 50)]" @keyup="emailKeyup" standout tabindex="2" />
      </div>
      <div class="col">
        <q-btn class="" color="secondary" text-color="black" size="lg" style="width: 160px;" @click="sendMailCode" tabindex="3">
          <b>{{ $t('send_email_code') }}</b>
        </q-btn>
      </div>
    </div>

    <!-- email check -->
    <div class="row justify-center q-pt-md">
      <div class="col-12">
        <span class="text-weight-bold text-subtitle1">{{ $t('enter_email_code') }}</span>
      </div>
    </div>
    <div class="row justify-left">
      <div class="col-6 q-pr-sm">
        <q-input v-model="code" ref="code" :rules="[val => minLength(val, 6), val => maxLength(val, 6)]" standout tabindex="4" />
      </div>
      <div class="">
        <q-btn class="" color="secondary" text-color="black" size="lg" style="width: 160px;" @click="checkEmailCode" tabindex="5">
          <b>{{ $t('verification') }}</b>
        </q-btn>
      </div>
    </div>

    <!-- pwd -->
    <div class="row justify-center q-pt-md">
      <div class="col-12">
        <span class="text-weight-bold text-subtitle1">{{ $t('user_pwd') }}</span>
      </div>
    </div>
    <div class="row justify-center">
      <div class="col-12">
        <q-input v-model="pwd" ref="pwd" type="password" :rules="[required, val => minLength(val, 6), val => maxLength(val, 30)]" clearable standout tabindex="6" />
      </div>
    </div>

    <!-- pwdCheck -->
    <div class="row justify-center q-pt-md">
      <div class="col-12">
        <span class="text-weight-bold text-subtitle1">{{ $t('user_pwd_check') }}</span>
      </div>
    </div>
    <div class="row justify-center">
      <div class="col-12">
        <q-input v-model="pwdCheck" ref="pwdCheck" type="password" :rules="[required, val => minLength(val, 6), val => maxLength(val, 30)]" clearable standout tabindex="7" />
      </div>
    </div>

    <div class="row justify-center q-pb-lg">
    </div>

    <!-- Buttons -->
    <div class="row justify-center q-pt-md">
      <div class="col-6 text-left">
        <q-btn class="btn" color="grey-3" text-color="black" size="lg" style="width: 98%;" @click="goBack" tabindex="9">
          <b>{{ $t('go_back') }}</b>
        </q-btn>
      </div>
      <div class="col-6 text-right">
        <q-btn class="btn" color="secondary" text-color="black" size="lg" style="width: 98%;" @click="registerUser" tabindex="8">
          <b>{{ $t('register') }}</b>
        </q-btn>
      </div>
    </div>

    <!-- ?????? ?????? ?????? -->
    <div class="row justify-center q-pa-xl">
    </div>

  </q-page>

  <q-dialog v-model="confirmGoBack">
    <q-card>
      <q-card-section class="row items-center" style="min-width: 200px;">
        <!-- <q-avatar icon="warning" color="primary" text-color="white" size="sm" /> -->
        <q-icon name="warning" color="primary" size="md" />
        <span class="q-ml-sm">{{ $t('confirm_go_back') }}</span>
      </q-card-section>
      <q-separator />
      <q-card-actions align="around">
        <q-btn flat style="width: 45%;" :label="$t('cancel')" color="black" v-close-popup />
        <q-btn flat style="width: 45%;" :label="$t('go_back')" color="black" v-close-popup @click="doGoBack" />
      </q-card-actions>
    </q-card>
  </q-dialog>

</template>

<script>
import { sha512 } from 'js-sha512'
import { defineComponent } from 'vue';
import { required, requiredNumber, minLength, maxLength, minValue, maxValue} from 'src/validation.js';

export default defineComponent({
  name: 'Join',
  data () {
    return {
      uid: '',
      code: '',
      pwd: '',
      pwdCheck: '',
      emailVerified: false,
      // auth_key: '',
      // nickname: '',
      // profile_image: '',
      // reg_name: '',
      // name: '',
      // mobile_no: '',
      // ucode: '',
      // birth: '',
      // gender: '',
      // nation: '',
      // tel_no: '',
      // thumbnail: '',
      // introduce: '',
      // push_agree_yn: '',
      // adcd: '',
      // id_card_image: '',
      // home_address: '',
      // home_address_image: '',
      // business_registration_no: '',
      // business_license_image: '',
      // company_address: '',
      // company_address_image: '',
      // company_tel_no: '',
      // reg_id: '',
      // reg_time: '',
      // mod_id: '',
      // mod_time: '',
      // del_id: '',
      // del_time: '',
      confirmGoBack: false, // goBack ?????????
    }
  },
  components: {
  },
  computed: {
    getWalletAddress () {
      return this.$store.getters.getWalletAddress
    },
    getWalletType () {
      return this.$store.getters.getWalletType
    },
    // getUid () {
    //   return this.$store.getters.getUid
    // }
  },
  created: function () {},
  mounted: function () {},
  methods: {
    ///////////////////////////////////////////////////////////////////////////
    // validation
    ///////////////////////////////////////////////////////////////////////////
    required(val) {
      const message = this.$t('validation_required')
      return required(val, message);
    },
    requiredNumber(val) {
      const message = this.$t('validation_required')
      return required(val, message);
    },
    minLength(val, length) {
      if (!val) {
        return true
      }
      const message = this.$t('validation_min_length') + ': ' + length
      return minLength(val, message, length);
    },
    maxLength(val, length) {
      if (!val) {
        return true
      }
      const message = this.$t('validation_max_length') + ': ' + length
      return maxLength(val, message, length);
    },
    minValue(val, value) {
      const message = this.$t('validation_min_value') + ': ' + value
      return minValue(val, message, value);
    },
    maxValue(val, value) {
      const message = this.$t('validation_max_value') + ': ' + value
      return maxValue(val, message, value);
    },
    ///////////////////////////////////////////////////////////////////////////
    checkEmail(param) {
      let regEmail = /^[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*\.[a-zA-Z]{2,10}$/
      return regEmail.test(param)
    },
    validate() {
      let result = true
      if (!this.$refs.wallet_address.validate()) {
        this.$noti(this.$q, this.$t('validation_failed_wallet_address'))
        result = false
      }
      // ID ????????? ?????? ??????
      if (!this.checkEmail(this.uid)) {
        this.$noti(this.$q, this.$t('email_must_be_email'))
        result = false
      }
      if (!this.$refs.uid.validate()) {
        this.$noti(this.$q, this.$t('validation_failed_uid'))
        result = false
      }
      if (!this.$refs.pwd.validate()) {
        this.$noti(this.$q, this.$t('validation_failed_pwd'))
        result = false
      }
      if (!this.$refs.pwdCheck.validate()) {
        this.$noti(this.$q, this.$t('validation_failed_pwd_check'))
        result = false
      }
      // ???????????? ?????? ??????
      if (this.pwd !== this.pwdCheck) {
        this.$noti(this.$q, this.$t('pwd_not_match'))
        result = false
      }
      // ????????? ?????? ??????
      if (!this.emailVerified) {
        this.$noti(this.$q, this.$t('email_not_verified'))
        result = false
      }
      return result
    },

    // ???????????? ?????? ?????????
    sendMailCode() {
      if (!this.checkEmail(this.uid)) {
        this.$noti(this.$q, this.$t('email_must_be_email'))
        return
      }
      const vo = {
        uid: this.uid
      }
      this.$q.loading.show()
      this.$axios.post('/api/login/sendMailCode', vo)
        .then((result) => {
          this.$q.loading.hide()
          // console.log(JSON.stringify(result.data))
          if (result.data && result.data.resultCd === 'SUCCESS') {
            // ???????????? ?????? ?????????
            this.$noti(this.$q, this.$t('mail_sent'))
          } else {
            this.$noti(this.$q, result.data.resultMsg)
          }
        })
        .catch((err) => {
          this.$q.loading.hide()
          console.log(err)
          this.$noti(this.$q, err)
        })
    },

    // ????????? keyup ????????? - ????????? emailVerified = false ??? ??????
    emailKeyup (event) {
      this.emailVerified = false
    },

    checkEmailCode() {
      if (!this.checkEmail(this.uid)) {
        this.$noti(this.$q, this.$t('email_must_be_email'))
        return
      }
      // ????????? ?????? ?????? ??????
      if (!this.code) {
        this.$noti(this.$q, this.$t('enter_email_verification_code'))
        return
      }
      if (!this.$refs.code.validate()) {
        this.$noti(this.$q, this.$t('check_email_verification_code'))
        return
      }

      // 1. ????????? ?????? ??????
      const userTempVo = {
        uid: this.uid,
        code: this.code,
      }
      this.$q.loading.show()
      this.$axios.post('/api/login/checkEmailCode', userTempVo)
        .then((result) => {
          if (result.data && result.data.resultCd === 'SUCCESS') {
            this.$q.loading.hide()
            this.$noti(this.$q, this.$t('email_code_verification_success'))
            this.emailVerified = true
          } else {
            this.$q.loading.hide()
            // ?????? ?????????
            this.$noti(this.$q, this.$t('email_code_verification_failed'))
          }
        })
        .catch((err) => {
          this.loading = false
          console.log(err)
          this.$noti(this.$q, err)
        })
    },

    // ???????????? ?????? ?????? ??????
    async registerUser() {
      // Field validation check
      if(!this.validate()) {
        // this.$noti(this.$q, this.$t('validation_failed'))
        return
      }

      // ???????????? ??????
      this.doRegisterUser()
    },
    // ???????????? ??????
    async doRegisterUser() {
      // 1. ???????????? ?????? ??????
      let encPwd = ''
      if (this.pwd) {
        encPwd = sha512(this.pwd)
      } else {
        encPwd = null
      }
      const params = {
        wallet_address: this.getWalletAddress,
        wallet_type: this.getWalletType,
        uid: this.uid,
        pwd: encPwd,
      }

      this.$q.loading.show() // ?????? ?????? ??????

      this.$axios.post('/api/user/insertUser', params)
        .then((result) => {
          // console.log(JSON.stringify(result.data))
          this.$q.loading.hide() // ?????? ?????? ??????
          if (result.data && result.data.resultCd === 'SUCCESS') {
            // console.log(result.data)
            this.$noti(this.$q, this.$t('register_user_success'))
            this.$router.push('/')
          } else {
            this.$noti(this.$q, this.$t('register_user_failed'))
          }
        })
        .catch((err) => {
          this.$q.loading.hide() // ?????? ?????? ??????
          console.log(err)
          this.$noti(this.$q, err)
        })
    },
    goBack() {
      // goBack ????????? ??????
      this.confirmGoBack = true
    },
    doGoBack() {
      this.$router.go(-1)
    }
  }
})
</script>

<style scoped>
</style>
