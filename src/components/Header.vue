<template>
  <div class="header">
    <img class="header__logo" src="/img/logo.svg">
    <div class="header__navigation"> 
      <span 
        class="navigation__item" 
        v-for="navigation in navigations" 
        :key="navigation.url"
        @click="openLink(navigation.url)"
      >
        {{ navigation.title }}
      </span>
    </div>
    <span class="header__phone">+7(000) 000-00-00</span>
    <img src="/img/menu.svg" class="header__burger" @click="openMenu = true">
    <Transition name="bounce">
      <div v-if="openMenu" class="header__menu">
        <div class="menu__header">
          <img class="menu__logo" src="/img/logo.svg">
          <img class="menu__close" src="/img/close.svg" @click="openMenu = false">
        </div>
        <div class="menu__navigatoins">
          <span 
            class="navigation__item" 
            v-for="navigation in navigations" 
            :key="navigation.url"
            @click="openLink(navigation.url)"
          >
            {{ navigation.title }}
          </span>
        </div>
        <div>
          <div class="menu__footer menu__footer--bold">
            <img class="footer__img" src="/img/phone.svg">
            +7(000) 000-00-00
          </div>
          <div class="menu__footer">
            <img class="footer__img" src="/img/mail.svg">
            Example@mail.ru
          </div>
        </div>
      </div>
    </Transition>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Header',
  data() {
    return {
      openMenu: false,
      navigations: []
    }
  },
  mounted() {
    axios({
      method: 'get',
      url: 'https://admin.skytech.choosedesign.ru/wp-json/basic/navigation',
    })
    .then((response) => {
      this.navigations = response.data
    });
  },
  methods: {
    openLink(url) {
      document.location.href = location.origin + url
    }
  }
}
</script>

<style lang="scss" scoped>
.header__logo{
  width: 256px;
}
.header__phone{
  color: #0055A4;
  font-size: 20px;
  line-height: 24px;
  font-weight: 700;
}
.header{
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 22px 80px;
}
.header__navigation{
  display: flex;
  justify-content: space-around;
}
.navigation__item{
  font-weight: 400;
  font-size: 20px;
  margin: 0 25px;
  color: #313131;
  padding: 2px 0;
  cursor: pointer;
  border-bottom: 1px solid transparent;

  &:hover{
    color: #0055A4;
    border-color: #0055A4;
  }
}
.header__burger {
  display: none;
}
.header__menu {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: #FFFFFF;
  padding: 14px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  z-index: 1;
  box-sizing: border-box;
}
.menu__header {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.menu__logo {
  width: 125px;
  height: 30px;
}
.menu__close {
  height: 22px;
  width: 22px;
  cursor: pointer;
}
.menu__navigatoins {
  display: flex;
  flex-direction: column;
  gap: 20px;
}
.menu__footer {
  display: flex;
  align-items: center;  
  color: #0055A4;
  font-size: 16px;
  line-height: 19px;
  margin-bottom: 10px;
}
.menu__footer--bold {
  font-weight: 700;
}
.footer__img {
  width: 18px;
  height: 18px;
  margin-right: 9px;
}
.bounce-enter-active {
  animation: bounce-in 0.3s ease-in-out;
}
.bounce-leave-active {
  animation: bounce-in 0.3s ease-in-out reverse;
}
@keyframes bounce-in {
  0% {
    left: 100%;
  }
  100% {
    left: 0;
  }
}
@media screen and (max-width: 1450px) {
  .header__logo {
    width: 180px;
  }
  .header__phone{
    font-size: 16px;
    line-height: 20px;
  }
  .navigation__item {
    font-size: 16px;
    margin: 0 20px;
  }
  .header{
    margin: 22px 50px;
  }
}
@media screen and (max-width: 1100px) {
  .header__logo {
    width: 120px;
  }
  .header__phone{
    font-size: 14px;
    line-height: 20px;
  }
  .navigation__item {
    font-size: 14px;
    margin: 0 15px;
  }
  .header{
    margin: 18px 20px;
  }
}
@media screen and (max-width: 860px) {
  .header__phone{
    display: none;
  }
}
@media screen and (max-width: 710px) {
  .header__navigation{
    display: none;
  }
  .header__burger {
    display: block;
  }
  .navigation__item {
    font-family: Nexa;
    font-weight: 700;
    font-size: 29px;
    line-height: 32px;
    width: max-content;
  }
}
</style>
