<template>
  <div class="gameWrapper">
    <h1 class="gameWrapper__title">
      Guess the 4-digit number
    </h1>
    <p class="gameWrapper__desc"> Try to guess the secret 4-digit number with unique digits from 1 to 9. </p>
    <table class="gameWrapper__table">
      <tr>
        <th class="gameWrapper__guess">Guess</th>
        <th class="gameWrapper__goods" @mouseover="isShownModal = true" @mouseout="isShownModal = false">Goods
          <p class="gameWrapper__goodsModal" v-if="isShownModal">Number of digits in the guess that are in the secret
            number but
            in a different position.</p>
        </th>
        <th class="gameWrapper__goods" @mouseover="isShownRules = true" @mouseout="isShownRules = false">Correct
          <p class="gameWrapper__goodsModal" v-if="isShownRules">Number of digits in the guess that
            are in the correct position.</p>
        </th>
      </tr>
      <tr class="gameWrapper__valuesList">
        <td class="gameWrapper__item gameValue"></td>
        <td class="gameWrapper__itemGoods gameValue"></td>
        <td class="gameWrapper__itemCorrect gameValue"></td>
      </tr>
      <tr class="gameWrapper__valuesList">
        <td class="gameWrapper__item gameValue"></td>
        <td class="gameWrapper__itemGoods gameValue"></td>
        <td class="gameWrapper__itemCorrect gameValue"></td>
      </tr>
      <tr class="gameWrapper__valuesList">
        <td class="gameWrapper__item"></td>
        <td class="gameWrapper__itemGoods gameValue"></td>
        <td class="gameWrapper__itemCorrect gameValue"></td>
      </tr>
      <tr class="gameWrapper__valuesList">
        <td class="gameWrapper__item"></td>
        <td class="gameWrapper__itemGoods gameValue"></td>
        <td class="gameWrapper__itemCorrect gameValue"></td>
      </tr>
      <tr class="gameWrapper__valuesList">
        <td class="gameWrapper__item"></td>
        <td class="gameWrapper__itemGoods gameValue"></td>
        <td class="gameWrapper__itemCorrect gameValue"></td>
      </tr>
      <tr class="gameWrapper__valuesList">
        <td class="gameWrapper__item"></td>
        <td class="gameWrapper__itemGoods gameValue"></td>
        <td class="gameWrapper__itemCorrect gameValue"></td>
      </tr>
      <tr class="gameWrapper__valuesList">
        <td class="gameWrapper__item"></td>
        <td class="gameWrapper__itemGoods gameValue"></td>
        <td class="gameWrapper__itemCorrect gameValue"></td>
      </tr>
      <tr class="gameWrapper__valuesList">
        <td class="gameWrapper__item"></td>
        <td class="gameWrapper__itemGoods gameValue"></td>
        <td class="gameWrapper__itemCorrect gameValue"></td>
      </tr>
      <tr class="gameWrapper__valuesList">
        <td class="gameWrapper__item"></td>
        <td class="gameWrapper__itemGoods gameValue"></td>
        <td class="gameWrapper__itemCorrect gameValue"></td>
      </tr>
      <tr class="gameWrapper__valuesList">
        <td class="gameWrapper__item"></td>
        <td class="gameWrapper__itemGoods gameValue"></td>
        <td class="gameWrapper__itemCorrect gameValue"></td>
      </tr>

    </table>
    <div v-if="isWinner || gameOver" :class="isWinner ? 'gameWrapper__winn' : 'gameWrapper__gameOver'">
      <p v-if="isWinner" class="gameWrapper__message">You won! The secret number is {{ secretNumber }}</p>
      <p v-else-if="gameOver" class="gameWrapper__message">Game over. The secret number was {{ secretNumber }}</p>
      <button @click="playAgain" class="gameWrapper__playAgainBtn">play again</button>
    </div>
    <div v-else>
      <form class="gameWrapper__setNumber" @submit.prevent="submitForm">
        <input type="text" class="gameWrapper__enterNumber" required placeholder="Enter your guess" maxlength="4"
               minlength="4" pattern="^(?!.*(.).*\1)[1-9]{1,4}$" v-model="enteredNumber">
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
const secretNumber = computed(() => {
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
  for (let i = 0; i < items.length; i++) {
    if (!items[i].textContent) {
      let entered = enteredNumber.value.split('')
      let sec = secretNumber.value.toString().split('')
      entered.forEach((item, idx) => {
        sec.forEach((num, i) => {
          if (item === num && idx === i) {
            correct.value++
          } else if (item === num && idx !== i) {
            goods.value++
          }
        })
      })
      if (correct.value === 4) {
        isWinner.value = true;
      } else if (i === items.length - 1) {
        gameOver.value = true;
      }
      items[i].textContent = enteredNumber.value
          // + ' ' + goods.value + ' ' + correct.value
      items[i].closest('tr').querySelector('.gameWrapper__itemGoods').textContent = goods.value.toString()
      items[i].closest('tr').querySelector('.gameWrapper__itemCorrect').textContent = correct.value.toString()
      correct.value = 0;
      goods.value = 0
      enteredNumber.value = '';
      return;
    }
  }
}


//reset all values
function resetNumbers() {
  let items = document.querySelectorAll('.gameValue');
  for (let item of items) {
    item.textContent = '';
  }
  isWinner.value = false;
}



//play again
function playAgain() {
  location.reload();
  return false;
}
</script>

<style lang="scss" src="./styles/style.scss"></style>
