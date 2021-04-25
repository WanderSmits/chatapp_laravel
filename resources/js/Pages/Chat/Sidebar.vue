<template>
    <div class="flex flex-col justify-between border h-full 1000px">
        <div class="p-6 cursor-pointer">
            <ul v-for="(room, index) in rooms" :key="index" :value="room">
                <li
                    :class="{
                        'text-gray-900': openChat === room.id,
                        'text-gray-400': openChat !== room.id,
                    }"
                    class="p-2"
                    @click="changeRoom(room)"
                >
                    {{ room.name }}
                </li>
            </ul>
        </div>

        <div class="relative px-12 bottom-16 cursor-pointer h-auto max-h-12">
            <h1>Add channel</h1>
            <font-awesome-icon icon="user-secret" />
            <font-awesome-icon :icon="['fas', 'user-secret']" />
        </div>
    </div>
</template>

<script>
export default {
    props: ["rooms", "currentRoom"],
    data() {
        return {
            openChat: 1,
        };
    },

    methods: {
        changeRoom(room) {
            this.$emit("roomchanged", room);
            this.openChat = room.id;
        },
    },

    watch: {
        selected: function (val) {
            //do something when the data changes.
            if (val) {
                console.log(this.selected);
            }
        },
    },
};
</script>
