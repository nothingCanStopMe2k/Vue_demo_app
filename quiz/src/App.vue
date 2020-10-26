<template>
  <div id="app">
    <Header :idquestionS = "index" /> 
    <b-container class="bv-example-row">
      <b-row>
        <!-- trong khoảng time dữ liệu đc response thì lúc nãy mảng questions chưa có gì, lúc này nếu chạy các ràng buộc bên dưới thì sẽ error -->
        <b-col sm="6" offset="3">

        <QuestionBox 
        v-if="questions.length"  
        :currentQuestion = "questions[index]"
        :next="next"/>

        </b-col>
      </b-row>
    </b-container>
  </div>
</template>


<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'

export default {
  name: 'App',
  components: {
    Header,
    QuestionBox
  },
  data(){
    return {
      questions: [],
      index: 0
    }
  },
  methods: {
    next(){
      this.index++
    }
  },

  //Fetch từ trên opentdb.com
  mounted: function(){
    fetch('https://opentdb.com/api.php?amount=10&type=multiple', {
      method: 'get'
    })
    .then ((res) => {
      return res.json()
    })
    .then((resData) => {
      this.questions = resData.results
    })
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 30px;
}
</style>
