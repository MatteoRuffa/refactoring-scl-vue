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
                <EmojiSelector 
                    :choice="feedbackData.choices"
                 />
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
        }
    },
    mounted() {
        //carico tutti i dati in feeddbackData
        console.log(2)
        this.loadData();
    },
};
</script>