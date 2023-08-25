<template>
    <div class="app__container">
        <div class="question__container">
            <input v-model="question" type="text" placeholder="Hazme una pregunta">s
            <p>Recuerda terminar con un signo de interrogación (?)</p>
        </div>
        <img v-if="isValidQuestion" class="img-tv" src="../assets/tv.png" alt="tv">
        <div v-if="isValidQuestion" class="response__container">
            <div v-if="img">
                <img :src="img" class="img-response" alt="response-img">
            </div>
            <div  class="response">
                <h1 class="globo globo-question">{{ question }}</h1>
                <h1 class="globo globo-answer">{{ answer }}</h1>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Indecision',
    data () {
        return {
            question: '',
            answer: null,
            img: null,
            isValidQuestion: false
        }
    },
    watch: {
        question (value, oldValue) {
            this.isValidQuestion = false
            if (!value.includes('?')) return
            this.isValidQuestion = true
            this.getAnswer(value)
        }
    },
    methods: {
        async getAnswer () {
            this.answer = 'Pensando...'
            const { answer, image } = await fetch('https://yesno.wtf/api').then(r => r.json())
            this.answer = answer === 'yes' ? 'La respuesta es... Sí!' : 'La respuesta es... No!'
            this.img = image
        }
    },
}
</script>