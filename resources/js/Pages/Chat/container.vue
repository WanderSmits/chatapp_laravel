<template>
    <app-layout>
        <!-- </template> -->
        <body>
            <div class="flex relative h-full-nav">
                <div class="grid grid-cols-6 flex-1">
                    <div class="bg-gray-100 col-span-1">
                        <sidebar
                            v-if="currentRoom.id"
                            :rooms="chatRooms"
                            :currentRoom="currentRoom"
                            v-on:roomchanged="setRoom($event)"
                        />
                    </div>
                    <div
                        class="col-span-5 bg-gray-50 overflow-hidden flex flex-col-reverse shadow-xl"
                    >
                        <!-- current room data is being passed to input message component-->
                        <input-message
                            :room="currentRoom"
                            v-on:messagesent="getMessage()"
                        />
                        <message-container :messages="messages" />
                    </div>
                </div>
            </div>
        </body>
    </app-layout>
</template>

<script>
import AppLayout from "@/Layouts/AppLayout";
import MessageContainer from "./messageContainer.vue";
import InputMessage from "./inputMessage.vue";
import Sidebar from "./Sidebar.vue";

export default {
    components: {
        AppLayout,
        MessageContainer,
        InputMessage,
        Sidebar,
    },
    data: function () {
        return {
            chatRooms: [],
            currentRoom: [],
            messages: [],
        };
    },
    methods: {
        getRooms() {
            axios
                .get("/chat/rooms")
                .then((res) => {
                    this.chatRooms = res.data;
                    this.setRoom(res.data[0]);
                })
                .catch((error) => {
                    console.log(error);
                });
        },
        setRoom(room) {
            //set room as current room data
            this.currentRoom = room;
            //whenever the room is set, get the messages for the current room
            this.getMessage();
        },
        getMessage() {
            axios
                .get("/chat/room/" + this.currentRoom.id + "/messages")
                .then((res) => {
                    console.log(res.data);
                    this.messages = res.data;
                })
                .catch((error) => {
                    console.log(error);
                });
        },
    },
    created() {
        this.getRooms();
    },
};
</script>
