<template>
<!--    {{message}}-->
<!--    {{msg}}-->
<!--    {{options}}-->
    <div>
        Current language: {{languages[current_language_id]}} <button v-on:click="current_language_id = (current_language_id + 1) % languages.length">Switch Language</button>
    </div>
    <div>
        <hr/>
    </div>
<!--    <div>-->
<!--        Question: <button v-on:click="get_question(languages[current_language_id], already_seen)">Get question</button>-->
<!--    </div>-->
    <div>
        {{questions[languages[current_language_id]][current_question_id]}}
    </div>
    <div>
        <button v-on:click="handle_question_answer('yes')" style="background-color:lightgreen">Yes</button>
<!--        <button v-on:click="handle_question_answer('no')" style="background-color: palevioletred">No</button>-->
        <button v-on:click="handle_question_answer('skip')">Doesn't matter</button>
    </div>
    <br/>
    <div>
        <div v-for="(data, name) in options" v-bind:key="name">{{name}}</div>
    </div>
</template>

<script>
    import data from './data2.json'
    import questions from './questions.json'
    export default {
        name: "Options",
        props: {
            message: String,
        },
        data() {
            return {
                msg: "This is a msg",
                options: data,
                questions: questions,
                languages: Object.keys(questions),
                current_language_id: 0,
                already_seen: ['inside'],
                current_question_id: 'inside'
            }
        },
        mounted() {
            window.addEventListener('keydown', (e)=> {
                if(e.key === 'ArrowLeft') {
                    this.handle_question_answer('yes');
                } else if (e.key === 'ArrowRight') {
                    this.handle_question_answer('skip');
                }
            })
        },
        methods: {
            get_question: function() {
                var question_options = questions[this.languages[this.current_language_id]]//.filter(question => this.already_seen.contains(question));
                this.already_seen.forEach(x => delete question_options[x]);
                var keys = Object.keys(question_options);
                this.current_question_id = keys[keys.length * Math.random() << 0];
            },
            handle_question_answer: function(answer) {
                console.log(this.options);
                var new_options = this.options;
                switch (answer) {
                    case 'yes':
                        new_options = Object.keys(this.options).reduce((acc, cur) => {
                            if (this.options[cur][this.current_question_id] === true) {
                                acc[cur] = this.options[cur]
                            }
                            return acc;
                        }, {});
                        break;
                    case 'no':
                        new_options = Object.keys(this.options).reduce((acc, cur) => {
                            if (this.options[cur][this.current_question_id] === false) {
                                acc[cur] = this.options[cur]
                            }
                            return acc;
                        }, {});
                        break;
                    case 'skip':
                        break;
                    default:
                        break;
                }
                this.already_seen.push(this.current_question_id);
                this.get_question(this.already_seen);
                this.options = new_options;
                console.log(this.options);
            }
        }
    }

</script>

<style scoped>

</style>