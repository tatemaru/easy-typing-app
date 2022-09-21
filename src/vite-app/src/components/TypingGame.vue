<template>
  <v-container>
    <div class=" text-center" v-if="questionViewFlg">
      <p class="text-h3">{{question.japanese}}</p>
      <p>
        <span class="text-h2" v-for="(item,index) in arrQuestion" :key="index" v-bind:class="{'red': item.flg === true}">
          {{ item.letter }}
        </span>
      </p>
    </div>
    <div class="text-center" v-if="clearViewFlg">
      <p class="text-h4">終了!</p>
      <v-btn
        elevation="2"
        @click="rePlay"
      >もう一度遊ぶ</v-btn>
    </div>
  </v-container>
</template>

<script>
import { onMounted, ref} from 'vue'
import questionData from '../assets/json/question.json';
import arrayHelper from "../helpers/arrayHelper.js";

export default {
  name: 'TypingGame',
  setup() {
    const {shuffle} = arrayHelper()
    let questionCount = 0
    let letterCount = 0
    let questionArray = []
    questionArray = shuffle(questionData)
    const inputParam = ref('')
    const arrQuestion = ref([])
    const questionViewFlg = ref(true)
    const clearViewFlg = ref(false)
    const question = ref({
      japanese: questionArray[questionCount].japanese,
      alphabet: questionArray[questionCount].alphabet
    })

    onMounted(() => {
      document.addEventListener('keydown', onKeyDown)
    })

    const init = () => {
      // 問題文に表示するアルファベットの配列を作る
      let arr = ref(question.value.alphabet.split(''))
      console.log(arr.value)
      let array = []
      for (let i=0; i<arr.value.length; ++i) {
        array[i] = {'letter':arr.value[i], 'flg':false}
      }
      arrQuestion.value = array
    }

    const onKeyDown = (event) => {
      if(event.key == arrQuestion.value[letterCount].letter){
        arrQuestion.value[letterCount].flg = true
        letterCount++
      }
      if (letterCount === arrQuestion.value.length) {
        letterCount = 0
        questionCount++
        if (questionCount === questionData.length) {
          document.removeEventListener('keydown', onKeyDown)
          questionViewFlg.value = false
          clearViewFlg.value = true
          return
        }
        question.value.japanese = questionArray[questionCount].japanese
        question.value.alphabet = questionArray[questionCount].alphabet
        init()
      }
    }
    const rePlay = () => {
      document.addEventListener('keydown', onKeyDown)
      letterCount = 0
      questionCount = 0
      questionViewFlg.value = true
      clearViewFlg.value = false
      init()
    }

    init()
    return {
      question,
      arrQuestion,
      inputParam,
      onKeyDown,
      questionViewFlg,
      clearViewFlg,
      rePlay
    }
  }
}
</script>

<style>
  .red {
    color: red;
  }
</style>