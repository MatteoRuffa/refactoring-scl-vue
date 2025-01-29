<template>
    <div class="feedback-page">
        <!-- <div v-if="store.loading" class="loading">
            <p>Caricamento dati...</p>
        </div>
        <div v-else> -->
            <!-- Titolo -->
            <div class="feedback-header" >
                <h1>Feedback - </h1>
            </div>

            <!-- Campi di feedback -->
            <div class="feedback-fields">
                <FeedbackField v-for="question in FeedbackPageData.list" :key="question.id"
                    :list="FeedbackPageData.list" 
                />
            </div> 

            <!-- Textarea per commenti -->
            <div class="feedback-comments">
                <h3>Hai un ulteriore suggerimento per noi?</h3>
                <TextArea
                    :placeholder="'Scrivi qui...'"
                    :maxLength="500"
                    v-model="FeedbackPageData.additionalComment"
                />
            </div>

            <!-- Pulsanti di navigazione -->
            <div class="feedback-footer">
                <NavigationsButtons 
                @back="goBack" 
                :choiceId="$route.params.choiceId"/>
            </div>
            <!-- @submit="submitFeedback()"  -->
        </div>
    <!-- </div> -->
</template>

<script>
import axios from "axios";
// import { startInactivityTimeout, resetInactivityTimeout } from "@/utils/GlobalTimeout";
import FeedbackField from "@/components/FeedbackField.vue";
import TextArea from "@/components/TextArea.vue";
import NavigationsButtons from "@/components/NavigationsButtons.vue";

export default {
    name: "FeedbackPage",
    components: { 
        FeedbackField,
        TextArea,
        NavigationsButtons },
    data() {
        return {
            FeedbackPageData: {},
        };
    },
    methods: {
        async loadData() {
            this.loading = true;
            this.error = null;

            try {
                const choiceId = this.$route.params.choiceId;
                const response = await axios.get(`http://localhost:3000/api/v1/scl/getChoices/${choiceId}`);
                this.FeedbackPageData = response.data;

                console.log("FeedbackPageData", this.FeedbackPageData);

            } catch (error) {
                this.error = "Errore nel caricamento dei dati";
                console.error("Errore nella richiesta API:", error); 
            } finally {
                this.loading = false;
            }
        },
        goBack() {
            this.$router.push("/");
        },

        //per tornare alla home page dopo 1 minuto di inattività
        // handleUserInteraction() {
        //     resetInactivityTimeout(this.$router, 60000);
        // },
        // //per inviare i dat al server e andare alla pag thankyou
        // submitFeedback() {
        //     // Passa il router al metodo submitFeedback dello store
        //     store.submitFeedback(this.$router); // Passa il router
        // },

    },
    mounted() {
        // startInactivityTimeout(this.$router, 60000);

        //carico tutti i dati in FeedbackPageData
        this.loadData();
    },
    watch: {
        // //controllo se l'utente ha interagito con la pagina
        // "store.feedbackData.questions": {
        //     handler() {
        //         this.handleUserInteraction();
        //     },
        //     deep: true,
        // },
        // //controllo se l'utente ha interagito con la textarea
        // "store.feedbackData.additionalComments"() {
        //     this.handleUserInteraction();
        // },
    }, computed: {
        // //genero la lista dei valori selezionati in base all id inviato
        // selectedChoiceList() {
        //     let choiceId = this.$route.params.choiceId;
        //     let selectedChoice = this.store.feedbackData.choices.find(choice => choice.id === choiceId);
        //     return selectedChoice ? selectedChoice.list : [];
        // },

        // //genero il titolo dinamico del feedback
        // selectedChoiceText() {
        //     let choiceId = this.$route.params.choiceId;
        //     let selectedChoice = this.store.feedbackData.choices.find(choice => choice.id === choiceId);
        //     return selectedChoice ? selectedChoice.text : "";
        // },

        // //sto provando a generare il textarea dinamico collegato all id della list
        // selectedChoiceAdditionalComment: {
        //     // Getter: recupera additionalComment per l'oggetto corretto
        //     get() {
        //         const choiceId = this.$route.params.choiceId;
        //         const selectedChoice = this.store.feedbackData.choices.find(
        //             (choice) => choice.id === choiceId
        //         );
        //         return selectedChoice ? selectedChoice.additionalComment : "";
        //     },
        //     // Setter: aggiorna additionalComment per l'oggetto corretto
        //     set(newValue) {
        //         const choiceId = this.$route.params.choiceId;
        //         const selectedChoice = this.store.feedbackData.choices.find(
        //             (choice) => choice.id === choiceId
        //         );
        //         if (selectedChoice) {
        //             selectedChoice.additionalComment = newValue;
        //         }
        //     },
        // },

        // selectedChoiceTitle() {
        //     const choiceId = this.$route.params.choiceId;
        //     const selectedChoice = this.store.feedbackData.choices.find(choice => choice.id === choiceId);
        //     return selectedChoice ? `bg-${selectedChoice.color}` : "bg-default";
        // }
    },
};
</script>

<style lang="scss" scoped>
.feedback-page {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 2rem;
    padding: 0 0;
    width: 100%;
    .bg-orange {
        background: radial-gradient(circle, #ff9c3a 50%, #ff8930 80%, #ff7e28 100%);
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.3);
    }
    .bg-red {
        background: radial-gradient(circle, #e64545 50%, #b83838 80%, #8a2c2c 100%);
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.3);
    }
    .feedback-header {
        width: 100vw;
        height: 80px;
        display: flex;
        justify-content: center;
        align-items: center;
        text-align: center;
        padding: 0;

        h1 {
            font-size: 2.0rem;
            font-weight: bold;
            color: #3c3c3c;
            margin: 0;
        }
    }
    .feedback-comments {
        width: 80vw;
        margin: 0 auto;
        text-align: center;
        padding-top: 20px;

        h3 {
            text-align: center;
            font-size: 1.6rem;
        }
    }
    .feedback-footer {
        width: 50vw;
        margin: 0 auto;
        padding-top: 10px;
    }
}

.loading {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100%;
    font-size: 1.2rem;
    color: #888;
}
</style>