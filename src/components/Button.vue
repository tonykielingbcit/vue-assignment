<script>
    export default {
        props: {
            button: Object,
        },
        emits: ["passData"],
        methods: {
            sendData() {
                const { value, type, text } = this.button;
                const dataToBeSent = { value, type, text };
                this.$emit("passData", dataToBeSent);
            }
        }
    };
</script>

<template>
    <div
        class="button ac"
        :class="this.button.class"
        :title = "this.button.value"
        @click="sendData"
    >
        {{ this.button.text }}
    </div>
</template>
