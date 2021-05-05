<template>
  <div class="wedding-invitation" :class="{ 'invitation-bounce':canOpen }">
    <div class="invitation-container" :class="{ 'invitation-down':isOpening }">
      <div class="invitation-cover">
        <div class="cover-content" :class="{'invitation-up':isOpening}">
          <div class="content-inside">
            <!-- <img class="content-inside-photo" src="../images/photo.jpg"> -->
            <div class="content-inside-photo">
              <b-carousel
                id="carousel-fade"
                fade
                :interval="4000"
                controls
                no-touch
                no-hover-pause
              >
                <b-carousel-slide :img-src="require('../images/photo01.jpeg')" />
                <b-carousel-slide :img-src="require('../images/photo02.jpeg')" />
                <b-carousel-slide :img-src="require('../images/photo03.jpeg')" />
                <b-carousel-slide :img-src="require('../images/photo04.jpeg')" />
                <b-carousel-slide :img-src="require('../images/photo05.jpeg')" />
                <b-carousel-slide :img-src="require('../images/photo06.jpeg')" />
                <b-carousel-slide :img-src="require('../images/photo07.jpeg')" />
                <b-carousel-slide :img-src="require('../images/photo08.jpeg')" />
                <b-carousel-slide :img-src="require('../images/photo09.jpeg')" />
                <b-carousel-slide :img-src="require('../images/photo10.jpeg')" />
              </b-carousel>
            </div>
            <div class="content-inside-letter">
              <p class="content-inside-letter-title"><b>곽요한 ♥ 김벼울</b></p>
              <br/>
              <p>별처럼 밝고 빛나는 여자가 있습니다.</p>
              <p>바다처럼 깊고 넓은 남자가 있습니다.</p>
              <p>이제 그 별은 바다를 비추고,</p>
              <p>바다는 별빛을 담고 함께 나아가려 합니다.</p>
              <p>저희의 아름다운 동행이 시작되는 날,</p>
              <p>그 시작을 함께해 주시고</p>
              <p>축복해 주시면 감사하겠습니다.</p>
              <br/>
            </div>
            <div class="content-inside-info">
              <div class="content-inside-info-datetime">
                <p class="content-inside-info-datetime-title"><b>언제</b></p>
                <p><b>2021년 6월 12일</b> 오후 5시</p>
                <p>{{ ddayTitle }}<b>{{ dday }}</b></p>
                <p class="content-inside-info-datetime-dday"></p>
              </div>
              <br/>
              <div class="content-inside-info-location">
                <p class="content-inside-info-location-title"><b>어디에서</b></p>
                <p><b>여의도웨딩컨벤션</b> 3층 그랜드볼룸홀</p>
                <p>
                  <span>서울 영등포구 여의대로 14 </span>
                  <b-button id="button-address" size="sm" class="mb-1" variant="outline" @click="doCopyAddress('서울 영등포구 여의대로 14')">
                    <b-icon icon="file-earmark-text" aria-hidden="true"></b-icon>
                  </b-button>
                  <b-popover placement="top" ref="popoverAddress" target="button-address" title="주소 복사 완료">
                    서울 영등포구 여의대로 14 (KT여의도사옥)
                  </b-popover>
                </p>
                <br/>
              </div>
            </div>
            <div class="content-inside-accounts">
              <p class="content-inside-accounts-title"><b>마음을 전하려면</b></p>
              <p>
                <span class="content-inside-accounts-to">신랑에게, </span>
                <span>국민 652702-04-347993 </span>
                <b-button id="button-yohan" size="sm" class="mb-1" variant="outline" @click="doCopyYohan('65270204347993')">
                  <b-icon icon="file-earmark-text" aria-hidden="true"></b-icon>
                </b-button>
                <b-popover placement="top" ref="popoverYohan" target="button-yohan" title="계좌번호 복사 완료">
                  KB국민 652702-04-347993 (곽요한)
                </b-popover>
              </p>
              <p>
                <span class="content-inside-accounts-to">신부에게, </span>
                <span> 국민 922902-01-189220 </span>
                <b-button id="button-byeoul" size="sm" class="mb-1" variant="outline" @click="doCopyByeoul('92290201189220')">
                  <b-icon icon="file-earmark-text" aria-hidden="true"></b-icon>
                </b-button>
                <b-popover placement="bottom" ref="popoverByeoul" target="button-byeoul" title="계좌번호 복사 완료">
                  KB국민 922902-01-189220 (김벼울)
                </b-popover>
              </p>
              <br/>
            </div>
             <div class="content-inside-gallery">
              <p class="content-inside-gallery-title"><b>일반 청첩장을 보려면</b></p>
              <p>
                <span>이건 너무 개발자같죠? 구경가기</span>
                <b-button id="button-yohan" size="sm" class="mb-1" variant="outline" @click="openGallery">
                  <b-icon icon="link45deg" aria-hidden="true"></b-icon>
                </b-button>
              </p>
              <br/>
            </div>
          </div>
        </div>
        <div class="cover-inside-left" :class="{'opening':isOpening}"></div>
        <div class="cover-inside-right" :class="{'opening':isOpening}"></div>
        <img class="cover-inside-seal" src="../images/seal.png" @click="openInvitation" :class="{'invitation-flight':isOpening}">
        <b-icon icon="code-slash" style="width: 25px; height: 25px; color: #5A5A5A;" aria-hidden="true" @click="closeInvitation" class="invitation-cover-close"></b-icon>
      </div>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import VueClipboard from 'vue-clipboard2'
import VueMoment from 'vue-moment'
import moment from 'moment'
import { BootstrapVue, BootstrapVueIcons, CarouselPlugin } from 'bootstrap-vue'
import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue-icons.min.css'

Vue.use(VueClipboard)
Vue.use(VueMoment, {moment})
Vue.use(BootstrapVue)
Vue.use(BootstrapVueIcons)
Vue.use(CarouselPlugin)

export default {
  props: ['canOpen'],
  data() {
    return {
      isOpening: false,
      wish: '',
      isFocused: false,
      hasEntered: false,
      ddayTitle: '',
      dday: ''
    }
  },
  created() {
    const today = moment(new Date()).format('YYYY-MM-DD')
    const eventDay = moment('2021-06-12', 'YYYY-MM-DD')
    const dDay = eventDay.diff(today, 'days')
    if (dDay == 0) {
      this.ddayTitle = "축복의 시간, "
      this.dday = "드디어 오늘"
    } else if (dDay < 0) {
      this.ddayTitle = "축복받은 후, "
      this.dday = Math.abs(dDay) + "일째"
    } else {
      this.ddayTitle = "축복의 시간까지, "
      this.dday = dDay + "일 남았음"
    }
  },
  methods: {
    // Open/Close the inviation
    openInvitation(){
      this.isOpening = true
    },
    closeInvitation() {
      this.isOpening = false
      setTimeout(() => {
        this.$emit('onClose')
      }, 660)
    },
    openGallery() {
      window.open("https://byeoul.com", "_blank");
    },
    doCopyAddress(message) {
      this.$copyText(message)
      this.$refs.popoverByeoul.$emit('close')
      this.$refs.popoverYohan.$emit('close')
      this.$refs.popoverAddress.$emit('open')
      setTimeout(() => {
        this.$refs.popoverAddress.$emit('close')
      }, 2000);
    },
    doCopyYohan(message) {
      this.$copyText(message)
      this.$refs.popoverByeoul.$emit('close')
      this.$refs.popoverAddress.$emit('close')
      this.$refs.popoverYohan.$emit('open')
      setTimeout(() => {
        this.$refs.popoverYohan.$emit('close')
      }, 2000);
    },
    doCopyByeoul(message) {
      this.$copyText(message)
      this.$refs.popoverAddress.$emit('close')
      this.$refs.popoverYohan.$emit('close')
      this.$refs.popoverByeoul.$emit('open')
      setTimeout(() => {
        this.$refs.popoverByeoul.$emit('close')
      }, 2000);
    }
  }
}
</script>

<style lang="less">
  .wedding-invitation{
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    padding: 20px 15px;
    padding-top: 40px;
    z-index: 4;
    transform: scale(0.05);
    -webkit-transform: scale(0.05);
    opacity: 0;
    transition: transform 0.8s cubic-bezier(.26,1.84,.39,.61), opacity 0.5s linear;
    -webkit-transition: -webkit-transform 0.8s cubic-bezier(.26,1.84,.39,.61), opacity 0.5s linear;
    background-size: 100%;
    overflow: hidden;
    &.invitation-bounce{
      opacity: 1;
      transform: scale(1);
      -webkit-transform: scale(1);
    }
    .invitation-container{
      position: relative;
      width: 100%;
      height: 100%;
      transition: transform 0.6s cubic-bezier(0.4, 0, 1, 1);
      -webkit-transition: -webkit-transform 0.6s cubic-bezier(0.4, 0, 1, 1);
      &.invitation-down{
        transform: translateY(20px);
        -webkit-transform: translateY(20px);
      }
      .invitation-cover{
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background-color: #DFD7DA;
        border-radius: 10px;
        perspective: 500px;
        box-shadow: 0 0 20px 2px rgba(0, 0, 0, 0.15);
        .invitation-cover-close {
          position: absolute;
          bottom: 20px;
          right: 20px;
        }
        .invitation-cover-close:hover {
          cursor: pointer;
        }
        .cover-content{
          position: absolute;
          top: 0;
          left: 0;
          width: 100%;
          height: 100%;
          padding: 10px 20px;
          z-index: 5;
          transition: transform 0.6s cubic-bezier(0.4, 0, 1, 1);
          -webkit-transition: -webkit-transform 0.6s cubic-bezier(0.4, 0, 1, 1);
          &.invitation-up{
            transform: translateY(-60px);
            -webkit-transform: translateY(-60px);
          }
          .content-inside{
            height: 100%;
            padding: 20px;
            color: #3E3E3E;
            background-color: #F1E6EB;
            overflow: auto;
            font-family: 'Nanum Myeongjo', serif;
            font-size: 13px;
            .content-inside-letter{
              text-align: center;
              .content-inside-letter-title {
                font-weight: bolder;
                margin-top: 30px;
                font-size: 17px;
              }
            }
            .content-inside-info {
              vertical-align: middle;
              margin-top: 15px;
              .content-inside-info-datetime {
                font-family: 'Gothic A1', sans-serif;
                font-weight: 300;
                .content-inside-info-datetime-title {
                  font-family: 'Nanum Myeongjo', serif;
                  font-size: 16px;
                  font-weight: 700;
                  margin-bottom: 7px;
                }
              }
              .content-inside-info-location {
                font-family: 'Gothic A1', sans-serif;
                font-weight: 300;
                .content-inside-info-location-title {
                  font-family: 'Nanum Myeongjo', serif;
                  font-size: 16px;
                  font-weight: 700;
                  margin-bottom: 6px;
                }
                p {
                  margin-bottom: -2px;
                }
              }
            }
            .content-inside-accounts {
              font-family: 'Gothic A1', sans-serif;
              font-weight: 300;
              .content-inside-accounts-title {
                font-family: 'Nanum Myeongjo', serif;
                font-size: 16px;
                margin-bottom: 1px;
                font-weight: 700;
              }
              .content-inside-accounts-to {
                font-weight: bold;
              }
              p {
                margin-bottom: -8px;
              }
            }
            .content-inside-gallery {
              font-family: 'Gothic A1', sans-serif;
              font-weight: 300;
              margin-top: 7px;
              .content-inside-gallery-title {
                font-family: 'Nanum Myeongjo', serif;
                font-size: 16px;
                margin-bottom: 2px;
                font-weight: 700;
              }
            }
            .content-inside-photo{
              width: 100%;
              margin-bottom: 10px;
            }
            p{
              margin-top: 0;
              margin-bottom: 5px;
            }
          }
        }
        .cover-inside-left{
          position: absolute;
          left: 0;
          top: 0;
          width: 70%;
          height: 100%;
          border-radius: 10px;
          background-color: #DFD7DA;
          box-shadow: 5px 0 10px rgba(0,0,0,0.2);
          z-index: 6;
          transition: transform 0.5s;
          -webkit-transition: -webkit-transform 0.5s;
          transform-origin: 0 50%;
          -webkit-transform-origin: 0 50%;
          &.opening{
            transform: rotate3d(0,1,0,-140deg);
            -webkit-transform: rotate3d(0,1,0,-140deg);
          }
        }
        .cover-inside-right{
          position: absolute;
          right: 0;
          top: 0;
          width: 40%;
          height: 100%;
          border-radius: 10px;
          background-color: #DFD7DA;
          box-shadow: -5px 0 10px rgba(0,0,0,0.2);
          z-index: 5;
          transition: transform 0.5s;
          -webkit-transition: -webkit-transform 0.5s;
          transform-origin: 100% 50%;
          -webkit-transform-origin: 100% 50%;
          &.opening{
            transform: rotate3d(0,1,0,140deg);
            -webkit-transform: rotate3d(0,1,0,140deg);
          }
        }
        .cover-inside-seal{
          position: absolute;
          left: 100%;
          bottom: 40px;
          width: 280px;
          margin-left: -280px;
          z-index: 7;
          transform-origin: 50% 50%;
          -webkit-transform-origin: 50% 50%;
          transition: all 0.8s cubic-bezier(0.4, 0, 1, 1);
          -webkit-transition: all 0.8s cubic-bezier(0.4, 0, 1, 1);
          &.invitation-flight{
            opacity: 0;
            left: 200%;
          }
        }
        .cover-inside-seal:hover {
          cursor: pointer;
        }
      }
    }
  }
</style>
