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
        allMessages.data = data.slice(0, 12);
    });

    // send message to api https://lab5-p379.onrender.com/api/v1/messages/ and add a user and a message
    const sendMessage = async () => {
    try {
        const response = await fetch("https://lab5-p379.onrender.com/api/v1/messages/", {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
            },
            body: JSON.stringify({
                user: 'Jarne',
                text: message.value,

            }),
        });

        console.log(response); // Log the entire response for debugging

        const data = await response.json();

        if (response.ok) {
            console.log(data);
            allMessages.data.push(data);
            message.value = "";
        } else {
            console.error("Error saving message:", data.message);
        }
    } catch (error) {
        console.error("Error:", error);
    }
};

   


</script>

<template>
    <div>
      <ul>
        <li v-for="m in allMessages.data" :key="m._id" class="message-item">
          <span class="username">{{ m.user }}:</span> {{ m.text }}
        </li>
      </ul>
      <div>
        <input v-model="message" type="text" placeholder="" />
        <button @click="sendMessage">Send</button>
      </div>
    </div>
  </template>

<style scoped>
.message-item {
  margin-bottom: 10px;
}

.username {
  font-weight: bold;
  margin-right: 5px;
}

li {
  list-style: none;
  
}

ul {
    padding: 0.5em 1em;
    
    background-color: #f5f5f5;
}

</style>
