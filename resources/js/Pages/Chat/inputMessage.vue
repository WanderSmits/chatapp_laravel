<template>
    <div class="px-4 flex w-width-input border">
        <input
            type="text"
            v-model="message"
            @keyup.enter="sendMessage()"
            placeholder="Say something"
            class="rounded-md flex-grow m-4 hover:border-gray-300"
        />
        <button
            @click="sendMessage()"
            class="text-gray-400 hover:text-gray-900 transition-colors"
        >
            Send
        </button>
    </div>
</template>

<script>
import Input from "../../Jetstream/Input";
export default {
    components: { Input },
    props: ["room"],
    data: function () {
        return {
            message: "",
        };
    },
    methods: {
        sendMessage() {
            // IF the message that is being send is empty, send nothing
            if (this.message == " ") {
                return;
            }

            axios
                .post(`/chat/room/${this.room.id}/message`, {
                    // post message to db
                    message: this.message,
                })
                .then((res) => {
                    if (res.status == 201) {
                        this.message = "";
                        // let parent component know that the message has been sent
                        this.$emit("messagesent");
                    }
                })
                .catch((error) => {
                    console.log(error);
                });
        },
    },
};
</script>
