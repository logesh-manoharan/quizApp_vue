<template>
    <div>
        <b-jumbotron>
            <h4>{{currentQuestion.question}} </h4>
            <hr>

            <b-list-group >
                <b-list-group-item @click="selectAnswer(index)" 
                :class="[!answered && selectedIndex === index ? 'selectedAnswer' : '',
                         answered && correctIndex === index ? 'correctAnswer': '',
                         answered && selectedIndex === index && correctIndex !== index  ? 'wrongAnswer' : '']" 
                v-for="(answer, index) in shuffledAnswers" :key="index">
                    {{answer}}
                </b-list-group-item>
            </b-list-group>
            <b-btn style="margin: 20px 30px 0px 0px;" variant="success" v-on:click="submitAnswer" :disabled="selectedIndex === null || this.answered">Submit</b-btn>
            <b-btn style="margin: 20px 0px 0px 30px;" variant="primary" v-on:click="next" :disabled="!answered">Next</b-btn>
        </b-jumbotron>
    </div>
</template>

<script>
import lodash from 'lodash';

export default {
    name: "QuizboxComponent",
    props: {
        currentQuestion: Object,
        next: Function,
        increment: Function,
        checkAttempt: Function
    },
    data() {
        return {
            correctIndex: null,
            shuffledAnswers: [],
            answered: false,
            selectedIndex: null
        }
    },
    //continously monitoring for each question [so, if we move on to next question this will execute]
    watch: {
        currentQuestion: {
            immediate: true,
            handler() {
                this.selectedIndex = null;
                this.answered = false;
                this.shuffleAnswers();
            }
        }
    },
    // We can also use like this. But, if some modifications are done by parent then it will be affected. So, better to use 'computed' property
    // created: function() {
    //     this.answers = this.currentQuestion.incorrect_answers;
    //     this.answers.push(this.currentQuestion.correct_answer);
    //     console.log("Answers: "+ this.answers);
    // },
    computed: {
        answers() {
            let answers = this.currentQuestion.incorrect_answers;
            answers.push(this.currentQuestion.correct_answer);
            return answers;
        }
    },
    methods: {
        selectAnswer(index) {
            this.selectedIndex = index;
        },
        shuffleAnswers() {
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];
            this.shuffledAnswers = lodash.shuffle(answers); 
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer);
        },
        submitAnswer() {
            let isCorrect = false;

            if(this.selectedIndex === this.correctIndex) {
                isCorrect = true;
            }
            this.answered = true;
            this.increment(isCorrect);
            this.checkAttempt();
        }
    }
}
</script>

<style scoped>
.jumbotron {
    width: 75%;
    margin: 0 150px 0 150px;
    text-align: center;
}

ul {
    list-style: none;
}

.list-group-item {
    cursor: pointer;
}

.list-group-item:hover {
    background: lightblue;
}

.selectedAnswer {
    background: #0275d8;
    color: white;
}

.correctAnswer {
    background: lightgreen;
}

.wrongAnswer {
    background: orange;
}
</style>