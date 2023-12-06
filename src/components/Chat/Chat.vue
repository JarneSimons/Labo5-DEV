<script setup>
    // import ref
    import { ref, reactive, onMounted } from 'vue';


    let message = ref(); // int, string, boolean

    // get data from this api https://lab5-p379.onrender.com/api/v1/messages/
    let allMessages = reactive({
        data: [],
    })

    // after loading page get data from api
    onMounted(async () => {
        const response = await fetch("https://lab5-p379.onrender.com/api/v1/messages/");
        const data = await response.json();
        data.reverse();
        allMessages.data = data.slice(0, 8);
    });


   


    const sendMessage = () => {
        allMessages.data.push(message.value);
        message.value = "";
    }

</script>

<template>
  <div>
    <ul>
        <li v-for="m in allMessages.data">{{ m }}</li>
    </ul>
    <div>
        <input v-model="message" type="text" placeholder="" />
        <button @click="sendMessage">Send</button>
    </div>
  </div>
</template>

<style scoped>
  
</style>
