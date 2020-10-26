<template>
    <div>
        <b-jumbotron>

            <template #lead>
                {{currentQuestion.question}}
     
            </template>

            <hr class="my-4">

            <b-list-group v-for="(answer, index) in answers" :key = "index">
                <b-list-group-item 
                :class="[select===index && !haveAnswer ? 'selected' : 
                correctIndex===index && haveAnswer ? 'correct': 
                select===index && haveAnswer ? 'incorrect' : ''
                ]" 
                @click="selectAnswer(index)"> {{answer}} </b-list-group-item>           
            </b-list-group>
        
            <b-button 
            variant="primary"
            @click="submitFunction()"
            :disabled="select === null"
            >Submit</b-button>
            <b-button @click="next" variant="success" href="#">Next</b-button>
        </b-jumbotron>
</div>
</template>

<script>
import _ from 'lodash'

export default {
    props: {
        currentQuestion:  Object,
        next: Function
    },
    data() {
        return {
            select: null,
            shuffleAnswers: [],
            correctIndex: null,
            haveAnswer: false
        }
    },  

    watch: { //Dùng để thay đổi trực tiếp trên dữ liệu trên trường data, ở đây là currentQuestion
        currentQuestion: {
            immediate: true,
            handler(){
                this.select = null
                this.shuffleAnswersfunction()
                this.haveAnswer = false,
                this.decodeHTML(this.currentQuestion.question)
                console.log(this.currentQuestion.question)
            }
        }

    },
    // Hàm để lấy câu hỏi lên
    computed: {
        answers(){
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]       
            return answers
        }
    },
    methods: {
        selectAnswer(index){
            this.select = index 
        },
        shuffleAnswersfunction(){
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
            this.shuffleAnswers = _.shuffle(answers) // Sử dụng function shuffle của npm lodash,
            this.correctIndex = this.shuffleAnswers.indexOf(this.currentQuestion.correct_answer) //Lấy id của correct_answer sau khi shuffle
        },
        
        submitFunction(){
            this.haveAnswer = true
        },

        decodeHTML(html){      
            var txt = document.createElement("textarea");
            txt.innerHTML = html;
            this.currentQuestion.question = txt.textContent
        }
    }
}
</script>
<style scoped>
    .list-group:nth-last-child(3){
        margin-bottom: 25px;
    }
    .list-group-item:hover{
        background: #EEE;
        cursor: pointer;
    }
    .incorrect{
        background-color: rgb(248, 78, 78);
    }
    .correct{
        background-color: lightgreen;
    }
    .selected{
        background-color: lightblue;
    }
    button  {
        margin-right: 1.5rem;

    }
</style>