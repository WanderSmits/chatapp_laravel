<template>
    <div class="px-8 py-4 overflow-y-auto flex flex-col-reverse">
        <div v-for="(message, index) in messages" :key="index">
            <message-item :message="message" />
        </div>
    </div>
</template>

<script>
import messageItem from "./messageItem.vue";

export default {
    components: { messageItem },
    props: ["messages"],
};
</script>
