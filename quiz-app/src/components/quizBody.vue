<template>
    <div class="quizBodyContainer">
        <div class="modalLeft">
            <!--qusetionContainer-->
            <div
                class="questionContainer"
                v-if="questionIndex<quiz.questions.length"
                :key="questionIndex"
            >
                <header>
                    <h1 class="title">Information Security Healthcheck Questionnaire</h1>
                    <div class="progressContainer">
                        <div>
                            Total {{ quiz.questions.length }}
                            Answered {{ questionIndex }}
                            Remaning {{ quiz.questions.length - questionIndex }}
                        </div>
                    </div>
                </header>

                <h1 class="titleContainer title">{{ quiz.questions[questionIndex].text }}</h1>
                <div class="optionContainer">
                    <div
                        class="option"
                        v-for="(response, index) in quiz.questions[questionIndex].responses"
                        @click="selectOption(index)"
                        :class="{ 'is-selected': userResponses[questionIndex] == index}"
                        :key="index"
                    >{{ response.text }}
                    </div>
                </div>

                <footer class="questionFooter">
                    <nav
                        class="pagination"
                        role="navigation"
                        aria-label="pagination"
                    >
                        <button
                            class="button"
                            @click="prev();"
                            ref="prevBtn"
                            :disabled="questionIndex <= 0 ? true : false"
                        >Back</button>
                        <button
                            class="button"
                            :class="(userResponses[questionIndex]==null)?'':'is-active'"
                            @click="next();"
                            ref="nextBtn"
                            :disabled="(userResponses[questionIndex]==null ? true : false)"
                        >{{ (quiz.questions.length === (questionIndex + 1)) ? 'Submit!' : 'Next' }}
                        </button>
                    </nav>
                </footer>
            </div>
            <!--/questionContainer-->

            <!--quizCompletedResult-->
            <div
                v-if="questionIndex >= quiz.questions.length"
                :key="questionIndex"
                class="quizCompleted"
            >
                <h2>Your score is:</h2>
                <h1 :class="results()">{{ score() }}</h1>
                <a
                    class="button"
                    @click="restart()"
                >Restart</a>
                <a class="button" @click="showModal=true">Send Email</a>
            </div>
            <!--/quizCompetedResult-->
        </div>

        <div class="modalRight">
            <img src="https://images.unsplash.com/photo-1512486130939-2c4f79935e4f?ixlib=rb-0.3.5&ixid=eyJhcHBfaWQiOjEyMDd9&s=dfd2ec5a01006fd8c4d7592a381d3776&auto=format&fit=crop&w=1000&q=80" alt="">
        </div>

        <!--/EmailModal-->
        <emailDialog
            v-if="showModal"
            @close="showModal = false"
        ></emailDialog>
        <!--/EmailModal-->
    </div>
</template>

<script>
    import emailDialog from './emailDialog.vue'
    let quiz = {
        questions: [
            {
                text: "Which of the following information security technology is used for avoiding browser-based hacking?",
                responses: [
                    { text: 'Anti-malware in browsers' },
                    { text: "Remote browser access", correct: true },
                    { text: "Adware remover in browsers" },
                    { text: "Incognito mode in a browser" }
                ]
            },
            {
                text: "The full form of EDR is ______",
                responses: [
                    { text: "Endpoint Detection and recovery" },
                    { text: "Early detection and response" },
                    { text: "Endpoint Detection and response", correct: true },
                    { text: "Endless Detection and Recovery" }
                ]
            },
           {
                text: "_______ technology is used for analyzing and monitoring traffic in network and information flow.",
                responses: [
                    { text: "Cloud access security brokers (CASBs)", correct: true },
                    { text: "Managed detection and response (MDR)" },
                    { text: "Network Security Firewall" },
                    { text: "Network traffic analysis (NTA)" }
                ]
            },
            {
                text: "Which of the following is defined as an attempt to steal, spy, damage or destroy computer systems, networks, or their associated information?",
                responses: [
                    { text: "Cyber attack", correct: true },
                    { text: "Computer security" },
                    { text: "Cryptography" },
                    { text: "Digital hacking" }
                ]
            },
            {
                text: "Possible threat to any information cannot be ________________",
                responses: [
                    { text: "reduced" },
                    { text: "transferred" },
                    { text: "Protected" },
                    { text: "Ignored", correct: true }
                ]
            },
            {
                text: "What are the features of cyber security?",
                responses: [
                    { text: "Compliance" },
                    { text: "Defence against internal threats" },
                    { text: "All of the above", correct: true },
                    { text: "Threat prevention" }
                ]
            },
            {
                text: "Which of the following is an objective of network security?",
                responses: [
                    { text: "Confidentiality" },
                    { text: "All of the above", correct: true },
                    { text: "Integrity" },
                    { text: "Availability" }
                ]
            },
            {
                text: "Which of these is not a proper method of maintaining confidentiality?",
                responses: [
                    { text: "Biometric verification" },
                    { text: "ID and password based verification" },
                    { text: "2-factor authentication" },
                    { text: "Switching off the phone", correct: true }
                ]
            },
            {
                text: "Lack of access control policy is a _____________",
                responses: [
                    { text: "Bug" },
                    { text: "Threat" },
                    { text: "Vulnerability", correct: true },
                    { text: "Attack" }
                ]
            },
            {
                text: "Which of the following is not a reconnaissance tool or technique for information gathering?",
                responses: [
                    { text: "Nexpose", correct: true },
                    { text: "NMAP" },
                    { text: "Google Dorks" },
                    { text: "Hping" }
                ]
            },
            {
                text: "From the options below, which of them is not a vulnerability to information security?",
                responses: [
                    { text: "Flood", correct: true },
                    { text: "Without deleting data, disposal of storage media" },
                    { text: "Unchanged default password" },
                    { text: "Latest patches and updates not done" }
                ]
            },
            {
                text: "Compromising confidential information comes under _________",
                responses: [
                    { text: "Bug" },
                    { text: "Threat", correct: true },
                    { text: "Vulnerability" },
                    { text: "Attack" }
                ]
            },
            {
                text: "One common way to maintain data availability is __________",
                responses: [
                    { text: "Data clustering", correct: true },
                    { text: "Data Recovery"},
                    { text: "Data backup" },
                    { text: "Data Altering" }
                ]
            },
            {
                text: "Which of the following do Cyber attackers commonly target for fetching IP address of a target or victim user?",
                responses: [
                    { text: "Ip tracker" },
                    { text: "Emails" },
                    { text: "Websites", correct: true },
                    { text: "Web pages" }
                ]
            },
            {
                text: "Which of the following is defined as an attempt to harm, damage or cause threat to a system or network?",
                responses: [
                    { text: "Digital crime" },
                    { text: "Threats" },
                    { text: "System hijacking" },
                    { text: "Cyber attack", correct: true }
                ]
            },
            {
                text: "IT security in any firm or organization is maintained and handled by ____________________",
                responses: [
                    { text: "Software Security Specialist" },
                    { text: "CEO of the organization" },
                    { text: "IT Security Engineer", correct: true },
                    { text: "Security Auditor" }
                ]
            },
            {
                text: "Which of the following online service’s privacy cannot be protected using Tor?",
                responses: [
                    { text: "Browsing data" },
                    { text: "Instant messaging" },
                    { text: "Login using ID" },
                    { text: "Relay chats", correct: true }
                ]
            },
            {
                text: "From the options below, which of them is not a threat to information security?",
                responses: [
                    { text: "Unchanged default password", correct: true },
                    { text: "Disaster" },
                    { text: "Eavesdropping" },
                    { text: "Information leakage" }
                ]
            },
            {
                text: "From the options below, which of them is not a vulnerability to information security?",
                responses: [
                    { text: "flood", correct: true },
                    { text: "without deleting data, disposal of storage media" },
                    { text: "unchanged default password" },
                    { text: "latest patches and updates not done" }
                ]
            },
            {
                text: "_____ platforms are used for safety and protection of information in the cloud.",
                responses: [
                    { text: "Cloud workload protection platforms" },
                    { text: "Cloud security protocols" },
                    { text: "AWS" },
                    { text: "One Drive", correct: true }
                ]
            },
        ]
    },
    userResponseSkelaton = Array(quiz.questions.length).fill(null);

    export default {
        components: {
            emailDialog
        },
        data() {
            return {
                quiz: quiz,
                questionIndex: 0,
                userResponses: userResponseSkelaton,
                isActive: false,
                disabled: 0,
                showModal:false
            }
        },
        methods: {
            restart: function(){
                this.questionIndex = 0;
                this.userResponses = Array(this.quiz.questions.length).fill(null);
            },
            selectOption: function(index) {
                this.userResponses[this.questionIndex] = index;
                this.$refs.nextBtn.removeAttribute('disabled');
            },
            next: function() {
                if (this.questionIndex < this.quiz.questions.length) {
                    this.questionIndex++;
                }
            },

            prev: function() {
                if (this.quiz.questions.length > 0) {
                    this.questionIndex--;
                }
            },

            score: function() {
                let score = 0;
                for (let i = 0; i < this.userResponses.length; i++) {
                    let correctAnswer = this.quiz.questions[i].responses[this.userResponses[i]].correct,
                        selectedAnswers =this.quiz.questions[i].responses[this.userResponses[i]]

                    if (typeof selectedAnswers !== "undefined" && correctAnswer) {
                        score = score + 1;
                    }
                }
                return Number((score / this.quiz.questions.length) * 100).toFixed(2);
            },
            results: function() {
               let score = 0;
                for (let i = 0; i < this.userResponses.length; i++) {
                    let correctAnswer = this.quiz.questions[i].responses[this.userResponses[i]].correct,
                        selectedAnswers =this.quiz.questions[i].responses[this.userResponses[i]]

                    if (typeof selectedAnswers !== "undefined" && correctAnswer) {
                        score = score + 1;
                    }
                }
                let corerectAns = Number((score / this.quiz.questions.length) * 100).toFixed(2);

                if (corerectAns >= 50 && corerectAns <= 80) {
                    return 'orange';
                } else if(corerectAns >= 80) {
                    return 'green';
                } else if (corerectAns < 50) {
                    return 'red';
                }
            },
        },
    };
</script>