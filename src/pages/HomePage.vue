<template>
    <div class="homepage">
        <div v-if="this.loading" class="loading">
            <p>Caricamento dati...</p>
        </div>
        <div v-else>
            <!-- Immagine della homepage -->
            <div class="homepage-image">
                <img :src="feedbackData.home.image" alt="Immagine della homepage" />
            </div>

            <!-- Titolo -->
            <div class="homepage-header">
                <h1>Come è stata la tua esperienza oggi?</h1>
            </div>

            <!-- Faccine -->
            <div class="emoji-container">
                <EmojiSelector :choice="feedbackData.choices" @select="selectChoice($event, this.$router)" />
            </div>
        </div>
    </div>
</template>

<script>
import EmojiSelector from "@/components/EmojiSelector.vue";
import axios from "axios";

export default {
    name: "HomePage",
    components: { EmojiSelector },
    data() {
        return {
            feedbackData: {},
            loading: true,
            error: null
        };
    },
    methods: {
        async loadData() {
            this.loading = true;
            this.error = null;

            try {
                const response = await axios.get("http://localhost:3000/api/v1/scl/getHome");
                this.feedbackData = response.data;
            } catch (error) {
                this.error = "Errore nel caricamento dei dati";
                console.error("Errore nella richiesta API:", error); //poi da togliere!
                console.log(1)
            } finally {
                this.loading = false;
            }
        },

        async selectChoice(choiceId, router) {
            let indexChoice = this.feedbackData.choices.findIndex((e) => e.id === choiceId);

            console.log("choiceId", choiceId);
            console.log("feedbackData.choices", this.feedbackData.choices);
            console.log("indexChoice", indexChoice);
            console.log("feedbackData.choices[indexChoice].list", this.feedbackData.choices[indexChoice].list);

            if (indexChoice !== -1) {
                this.feedbackData.choices[indexChoice].selected = 1;
            }
            if (this.feedbackData.choices[indexChoice].list == false) {
                try {
                    const response = await axios.post("http://localhost:3000/api/v1/scl/addFeedbackData",
                        this.feedbackData, 
                        {
                            headers: {
                                'Content-Type': 'application/json'
                            }
                        }
                    );
                    console.log("Feedback inviato con successo!", response.data);
                } catch (error) {
                    console.error("Errore nell'invio del feedback:", error);
                }
                router.push("/thank-you");
            }  else {
                router.push(`/page-feedback/${choiceId}`);  
            }
        }
    },
    mounted() {
        //carico tutti i dati in feeddbackData
        console.log(2)
        this.loadData();
    },
};
</script>

<style lang="scss" scoped>
.homepage {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  padding: 2rem;
  padding: 0;
  margin: 0;

  .homepage-image {
    flex: 0 0 60%;
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 1rem;
    border-radius: 5px;
    overflow: hidden;
    box-shadow:   0 4px 6px rgba(0, 0, 0, 0.1);

    img {
      max-height: 70vh;
      max-width: 100%;
      object-fit: cover;
    }
  }

  .homepage-header {
    flex: 0 0 auto;
    text-align: center;
    margin: 1rem 0;

    h1 {
      font-size: 2.0rem;
      font-weight: bold;
      color: #333;
    }
  }


}
  
  .loading {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100%;
    font-size: 1.5rem;
    color: #888;
  }
  </style>
  