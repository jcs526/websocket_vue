<template>
  <div>
    {{ msg }}
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from 'vue';
import type { Ref } from 'vue';
import { io } from "socket.io-client";
import axios from "axios";

const msg: Ref<string | number> = ref("");
msg.value = "메세지입니다."

interface State {
  connected: boolean,
  fooEvents: string[],
  barEvents: string[]
}

const state: Ref<State> = ref({
  connected: false,
  fooEvents: [],
  barEvents: []
});

const socket = io("http://localhost:3000");

onMounted(async () => {
  connect();
  socket.on("test", (message) => {
    console.log(message);
    msg.value = message;
  })
});


const connect = () => {
  console.log("connet method");
  socket.connect();
  socket.emit("message", "message!!!!")
}
const disconnect = () => {
  console.log("connet method");
  socket.disconnect();
}
onBeforeUnmount(async () => {
  disconnect();
});

</script>
<style scoped></style>
