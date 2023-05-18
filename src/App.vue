<template>
  <div class="gameWrapper">
    <h1 class="gameWrapper__title">
      Guess the 4-digit number  {{ secretNumber }}
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
    <div v-if="isWinner || gameOver" :class="isWinner ? 'gameWrapper__winn' : 'gameWrapper__gameOver'">
      <p v-if="isWinner" class="gameWrapper__message">You won! The secret number is {{ secretNumber }}</p>
      <p v-else-if="gameOver" class="gameWrapper__message">Game over. The secret number was {{ secretNumber }}</p>
      <button @click="playAgain" class="gameWrapper__playAgainBtn">play again</button>
    </div>
    <div v-else>
      <form class="gameWrapper__setNumber" @submit.prevent="submitForm">
        <input type="text" class="gameWrapper__enterNumber" required placeholder="Enter your guess" maxlength="4" minlength="4" pattern="^(?!.*(.).*\1)[1-9]{1,4}$" v-model="enteredNumber">
        <button class="gameWrapper__btn">
          <svg xmlns="http://www.w3.org/2000/svg" width="18" height="24" viewBox="0 0 24 24" fill="none" stroke="#fff"
               stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <path d="M22 2L11 13"></path>
            <path d="M22 2L15 22L11 13L2 9L22 2Z"></path>
          </svg>
        </button>
      </form>
      <button class="gameWrapper__reset" @click="resetNumbers">
        Reset
      </button>
    </div>

  </div>
</template>

<script setup>
import {computed, ref} from 'vue';
const isShownModal = ref(false)
const isShownRules = ref(false)
const enteredNumber = ref()
const goods = ref(0)
const correct = ref(0)
const isWinner = ref(false)
const gameOver = ref(false)

//randomly generated secret number
const secretNumber = computed(()=> {
    let digits = ['1', '2', '3', '4', '5', '6', '7', '8', '9'];
    let result = '';
    let num = 0;

    while (num < 4) {
      let index = Math.floor(Math.random() * digits.length);
      result += digits[index];
      digits.splice(index, 1);
      num++;
    }

    return result;
})

//add guessed number
function submitForm() {
  let items = document.querySelectorAll('.gameWrapper__item');
  for(let i = 0; i < items.length; i++) {
    if(!items[i].textContent) {
      let entered = enteredNumber.value.split('')
      let sec = secretNumber.value.toString().split('')
      entered.forEach((item, idx) => {
        sec.forEach((num, i) => {
          if(item === num && idx === i) {
            correct.value++
          }
          else if(item === num && idx !== i){
            goods.value++
          }
        })
      })
      if(correct.value === 4) {
        isWinner.value = true;
      }else if(i === items.length - 1) {
        gameOver.value = true;
      }
      items[i].textContent = enteredNumber.value + ' ' + goods.value + ' ' +   correct.value
      correct.value = 0;
      goods.value = 0
      enteredNumber.value = '';
      return;
    }
  }
}


//reset all values
function resetNumbers() {
  let items = document.querySelectorAll('.gameWrapper__item');
  for(let item of items) {
    item.textContent = '';
  }
  isWinner.value =false;
}


//play again
function playAgain() {
  location.reload();
  return false;
}
</script>

<style lang="scss" src="./styles/style.scss"></style>
