<script>
    import externalButtons from "../utils/calculator-bonus-03-button-data.js";
    import Button from "./Button.vue";

    export default {
        components: {
            Button
        },
        emits: ["sendDataToCalculator"],
        data() {
            return {
                buttons: externalButtons
            }
        },
        methods: {
            receiveData(data) {
                this.$emit("sendDataToCalculator", data)
            }
        }
    }
    
</script>

<template>
    <div class="calc-grid">
        <div v-for="(button, i) in this.buttons" :key="i" :class="button.class">
            <Button :button="button" :key="i" @passData="incoming => receiveData(incoming)"/>
        </div>
    </div>
</template>
