<template>
  <div class="slider">
    <hooper ref="carousel" @slide="updateCarousel">
      <slide class="slide" v-for="slide in slides" :key="slide.id">
        <img class="slide__main" :src="slide.img">
        <div class="slide__content">
          <div class="slide__title">{{slide.title}}</div>
          <div class="slide__description">{{ slide.description }}</div>
          <button class="slide__button">
            <div class="button__circle">
              <img class="circle" src="/img/circle.svg">
              <img class="button__plain" src="/img/flight_black.svg">
            </div>
            <div class="button__text">Читать подробнее</div>
          </button>
          <div class="slide__block-photo">
            <img class="block-photo__img" :src="slide.img">
            <button class="block-photo__button">
              Читать подробнее
            </button>
          </div>
        </div>
      </slide>
      <hooper-pagination class="slider__navigation--mobile" slot="hooper-addons"></hooper-pagination>
    </hooper>
    <div class="slider__navigation">
      <div 
        class="navigation__item" 
        :class="carouselData === slide.id ? 'active' : ''"
        v-for="slide in slides" 
        :key="slide.id" 
        @click="navigationClick(slide.id)"
      >
        <img class="navigation__img" :src="slide.img">
        <div class="navigation__timer" v-if="carouselData === slide.id && showTimer">
          <img class="timer__circle" src="/img/timer_circle.svg">
          <div>{{ timerCount }}</div>
          <div>сек</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { Hooper, Slide, Pagination as HooperPagination } from 'hooper';
import 'hooper/dist/hooper.css';
export default {
  name: 'Slider',
  components: {
    Hooper,
    Slide,
    HooperPagination
  },
  data () {
    return {
      carouselData: 0,
      timerCount: 5,
      showTimer: true,
      timer: null,
      autoChange: true,
      slides: [
        {
          id: 0,
          title: "Техническое обслуживание воздушных судов",
          description: "Современные технологии достигли такого уровня, что синтетическое тестирование напрямую зависит от новых предложений. А ещё тщательные исследования конкурентов подвергнуты.",
          img: "/img/plain.jpg"
        },
        {
          id: 1,
          title: "Новый партнёр отдела технического обслуживания ",
          description: "В рамках спецификации современных стандартов, независимые государства лишь добавляют фракционных разногласий и объединены в целые кластеры себе подобных. ",
          img: "/img/plain2.jpg"
        },
        {
          id: 2,
          title: "Набираем в штат более 15 новых специалистов",
          description: "С учётом сложившейся международной обстановки, современная методология разработки представляет собой интересный эксперимент проверки экспериментов",
          img: "/img/plain3.jpg"
        }
      ]
    }
  },
  mounted() {
    this.$nextTick(() => {
      this.autoChange = false
    })
    this.timer = setInterval(() => {
      if (this.timerCount !== 1) {
        this.timerCount --
      } else {
        this.autoChange = true
        if (this.carouselData === this.slides.length -1) {
          this.carouselData = 0
        } else {
          this.carouselData ++
        }
        this.timerCount = 5
        this.$nextTick(() => {
          this.autoChange = false
        })
      }
    }, 1000)
  },
  watch: {
    carouselData () {
      this.$refs.carousel.slideTo(this.carouselData);
    }
  },
  methods: {
    navigationClick(id) {
      this.carouselData = id;
      clearInterval(this.timer); 
      this.showTimer = false
    },
    updateCarousel(payload) {
      if (!this.autoChange) {
        this.carouselData = payload.currentSlide;
        clearInterval(this.timer); 
        this.showTimer = false
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.slide__block-photo {
  display: none;
}
.slider {
  height: 700px;
  position: relative;
  margin-bottom: 82px;
}
.hooper {
  height: 100%;
}
.slide {
  position: relative;
}
.slide__main{
  width: 100%;
  height: 100%;
  object-fit: cover;
}
.slide__content {
  position: absolute;
  left: 75px;
  top: 144px;
  color: #FFFFFF;
}
.slide__title{
  font-weight: 700;
  font-size: 70px;
  line-height: 70px;
  width: 961px;
  font-family: Nexa;
}
.slide__description{
  margin-top: 50px;
  font-weight: 400;
  font-size: 20px;
  line-height: 28px;
  width: 743px;
}
.slide__button {
  border: none;
  outline: none;
  background-color: transparent;
  font-family: Inter;
  position: relative;
  width: 200px;
  height: 200px;
  color: #FFFFFF;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  text-transform: uppercase;
  font-weight: 700;
  font-size: 14px;
  line-height: 17px;
  margin-top: 50px;

  &:hover {
    .button__circle {
      animation: move 2s ease-in-out;
    }
  }
}
.button__circle {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
.circle {
  width: 100%;
  height: 100%;
}
.button__plain {
  position: absolute;
  top: calc(50% - 7px);
  right: -10px;
}
.slider__navigation {
  position: absolute;
  bottom: 0;
  right: 75px;
  display: flex;
  transform: translate(0, 50%);
}
.navigation__item {
  margin: 0 20px;
  width: 260px;
  height: 165px;
  border-radius: 10px;
  border: 2px solid transparent;
  position: relative;
  filter: brightness(70%);

  &.active {
    filter:brightness(100%);
    border-color: #A0CAE2;
  }
}
.navigation__img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 10px;
}
.navigation__timer {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 60px;
  height: 60px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-family: Roboto;
  font-weight: 600;
  font-size: 16px;
  line-height: 13px;
  color: #FFFFFF;
}
.timer__circle {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  animation: move 3s linear infinite;
}
.slider__navigation--mobile {
  display: none;
}
@keyframes move {
  0% {
    transform: rotate(0);
  }
  100% {
    transform: rotate(-360deg);
  }
}
@media screen and (max-width: 1450px) {
  .slider {
    height: 600px;
    margin-bottom: 70px;
  }
  .slide__content {
    top: 80px;
  }
  .slide__title {
    font-size: 50px;
    line-height: 50px;
    width: 700px;
  }
  .slide__description {
    font-size: 18px;
    line-height: 24px;
    margin-top: 40px;
    width: 550px;
  }
  .slide__button {
    margin-top: 40px;
    width: 180px;
    height: 180px;
  }
  .slider__navigation {
    right: 60px;
  }
  .navigation__item {
    width: 240px;
    height: 140px;
  }
}
@media screen and (max-width: 1100px) {
  .slider {
    height: 500px;
    margin-bottom: 40px;
  }
  .slide__content {
    top: 60px;
  }
  .slide__title {
    font-size: 40px;
    line-height: 40px;
    width: 550px;
  }
  .slide__description {
    font-size: 16px;
    line-height: 22px;
    margin-top: 40px;
  }
  .slide__button {
    margin-top: 40px;
    width: 160px;
    height: 160px;
  }
  .slider__navigation {
    right: 30px;
  }
  .navigation__item {
    width: 180px;
    height: 100px;
  }
}
@media screen and (max-width: 860px) {
  .slider {
    height: 400px;
    margin-bottom: 40px;
  }
  .slide__content {
    top: 30px;
    left: 30px;
  }
  .slide__title {
    font-size: 28px;
    line-height: 28px;
    width: 400px;
  }
  .slide__description {
    font-size: 16px;
    line-height: 22px;
    margin-top: 20px;
  }
  .slide__button {
    margin-top: 20px;
    width: 140px;
    height: 140px;
  }
  .slider__navigation {
    right: 20px;
  }
  .navigation__item {
    width: 140px;
    height: 80px;
  }
}
@media screen and (max-width: 650px) {
  .slider {
    height: auto;
  }
  .slider__navigation--mobile {
    display: block;
    bottom: -30px;
    left: 20px;
    width: calc(50% - 20px);
    transform: none;
  }
  .slider__navigation {
    display: none;
  }
  .slide__main {
    display: none;
  }
  .slide__button {
    display: none;
  }
  .slide__content {
    color: #313131;
    width: 100%;
    position: static;
  }
  .slide__title {
    width: calc(100% - 30px);
    margin: 0 15px 20px 15px;
  }
  .slide__description {
    width: calc(100% - 30px);
    margin: 0 15px 20px 15px;
  }
  .slide__block-photo {
    display: block;
    position: relative;
    width: 100%;
    height: 234px;
  }
  .block-photo__img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }
  .block-photo__button {
    position: absolute;
    bottom: 30px;
    right: 20px;
    width: 110px;
    height: 100px;
    transform: translate(0, 50%);
    border: 1px solid #A0CAE2;
    background-color: #FFFFFF;
    font-family: Inter;
    font-weight: 700;
    font-size: 12px;
    text-transform: uppercase;
    color: #0055A4;
    border-radius: 50%;
    cursor: pointer;

    &:hover {
      background-color: #F4F8FA;
    }
  }
}
</style>

<style lang="scss">
  .hooper-indicator {
    width: 80px;
    background-color: #A0CAE2;

    &.is-active {
      background-color: #0055A4;
    }
  }

  @media screen and (max-width: 650px) {
    .hooper-indicator {
      width: 40px;
    }
    .hooper-track {
      padding-bottom: 50px;
    }
    .slider__navigation--mobile {
      bottom: 10px !important
    }
  }
</style>