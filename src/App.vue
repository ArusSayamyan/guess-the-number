<template>
  <div class="gameWrapper">
    <h1 class="gameWrapper__title">
      Guess the 4-digit number
    </h1>
    <p class="gameWrapper__desc"> Try to guess the secret 4-digit number with unique digits from 1 to 9. </p>
    <div class="gameWrapper__sections">
      <p class="gameWrapper__guess">Guess</p>
      <div class="gameWrapper__values">
        <p class="gameWrapper__goods" @mouseover="isShownModal = true" @mouseout="isShownModal = false">Goods</p>
        <p class="gameWrapper__goods" @mouseover="isShownRules = true" @mouseout="isShownRules = false">Correct</p>
        <p class="gameWrapper__goodsModal" v-if="isShownModal">Lorem ipsum dolor sit amet, consectetur adipisicing elit. Ad aliquid architecto minima mollitia nemo omnis totam. Explicabo facere laboriosam</p>
        <p class="gameWrapper__goodsModal" v-if="isShownRules">Lorem ipsum dolor sit amet, consectetur adipisicing elit.m</p>
      </div>
    </div>
    <ul class="gameWrapper__valuesList">
      <li class="gameWrapper__item"></li>
      <li class="gameWrapper__item"></li>
      <li class="gameWrapper__item"></li>
      <li class="gameWrapper__item"></li>
      <li class="gameWrapper__item"></li>
      <li class="gameWrapper__item"></li>
      <li class="gameWrapper__item"></li>
      <li class="gameWrapper__item"></li>
      <li class="gameWrapper__item"></li>
      <li class="gameWrapper__item"></li>
    </ul>
    <form class="gameWrapper__setNumber">
      <input type="number" class="gameWrapper__enterNumber" required placeholder="Enter your guess" maxlength="4" minlength="4" pattern="^(?!.*(.).*\1)[1-9]{1,4}$">
      <button class="gameWrapper__btn">
        <svg xmlns="http://www.w3.org/2000/svg" width="18" height="24" viewBox="0 0 24 24" fill="none" stroke="#fff"
             stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
          <path d="M22 2L11 13"></path>
          <path d="M22 2L15 22L11 13L2 9L22 2Z"></path>
        </svg>
      </button>
    </form>
    <button class="gameWrapper__reset">
      Reset
    </button>
  </div>
</template>

<script setup>
import {ref} from 'vue';
const isShownModal = ref(false)
const isShownRules = ref(false)
</script>

<style lang="scss" src="./styles/style.scss"></style>
