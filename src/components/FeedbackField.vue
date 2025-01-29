<template>
    <div class="feedback-field">
        <p class="feedback-question">{{ list.text }}</p> -->
        <EmojiSelector :choice="list.value" size="small" />
    </div>
</template>
<!-- @select="onSelect" -->

<script>
import EmojiSelector from "@/components/EmojiSelector.vue";

export default {
    name: "FeedbackField",
    props: {
        list: {
            type: Array,
            required: true,
        },
        // choiceId: {
        //     type: String,
        //     required: true,
        // },
        // questionId: {
        //     type: String,
        //     required: true,
        // },
    },
    components: { EmojiSelector },
    data() {
        return {
        };
    },
    methods: {
        //   onSelect(selectedValue) {
        //     console.log("Ho ricevuto la selezione:", selectedValue);
        //     // Aggiorna il "value" della domanda nello store
        //     store.updateFieldValue(this.choiceId, this.questionId, selectedValue);
        //   },
    },
    mounted() {
        console.log(" findValues():",this. findValues);
    },
    computed: {
        findValues() {
           
        }
    },
};
</script>

<style lang="scss" scoped>
.feedback-field {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 5rem;
    padding-left: 1.25rem;
    margin: 1.25rem 0px;

    .feedback-question {
        font-size: 1.2rem;
        margin: 0px;
        flex: 0 0 40%;
    }

    .emoji-selector {
        flex: 0 0 40%;
    }
}
</style>