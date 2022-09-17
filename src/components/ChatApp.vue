<template>
  <link rel="stylesheet" href="https://www.w3schools.com/w3css/4/w3.css" />
  <link
    href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.1/dist/css/bootstrap.min.css"
    rel="stylesheet"
    integrity="sha384-iYQeCzEYFbKjA/T2uDLTpkwGzCiq6soy8tYaI1GyVh/UjpbCx/TYkiZhlZB6+fzT"
    crossorigin="anonymous"
  />
  <!-- <div v-if="!joined" class="parent-container">
    <div class="name-container">
      <input type="text" class="user-name" v-model="currentUser" />
      <button class="join-button" v-on:click="join">Join</button>
    </div>
  </div> -->
  <center>
    <h1>Real Time Chat App</h1>
  </center>

  <div v-if="!joined" class="w3-container">
    <form @submit.prevent="join">
      <center>
        <h1>Please Enter Your Name to Enter in the ChatBox</h1>
        <hr />
        <label for="firstname"> Enter Your Name </label>
        <input
          class="nameClass"
          type="text"
          id="firstname"
          name="ftname"
          placeholder="Name"
          size="15"
          required
          v-model.trim="currentUser"
        />
        <button class="submit">Login</button>
      </center>

      <button type="reset" class="reset">Reset</button>
    </form>
  </div>
  
  <div v-if="joined">
    <center>   
      <div class="vertical-menu">
        <div class="message" v-for="message in messages" :key="message.id">
          <b>
            {{ message.user }}
          </b>
          : {{ message.text }}
        </div>
      </div>
    </center>

    <div>
      <!-- <input
        type="text"
        v-model.trim="text"
        class="text-message"
        v-on:keyup.enter="sendMessage"
      /> -->
      <button class="btn" @click="sendMessage">Send Msg</button>
      <textarea
        v-model="text"
        class="text-message"
        v-on:keyup.enter="sendMessage"
      ></textarea>
    </div>
  </div>
</template>

<script>
export default {
  name: "ChatApp",
  data() {
    return {
      joined: false,
      currentUser: "",
      text: "",
      messages: [],
    };
  },

  methods: {
    join() {
      this.joined = true;
      this.addMessage();
    },
    sendMessage() {
      this.addMessage();
      if (this.text != "") {
        fetch(
          "https://my-chat-application-4cbad-default-rtdb.firebaseio.com/messsages.json",
          {
            method: "POST",
            headers: {
              "Content-type": "application/json",
            },
            body: JSON.stringify({
              name: this.currentUser,
              msg: this.text,
            }),
          }
        );
        this.addMessage();
      }

      this.text = "";
    },

    addMessage() {
      // const message = {
      //   id: new Date().getTime(),
      //   text: this.text,
      //   user: this.currentUser,
      // };

      // this.messages = this.messages.concat(message);

      fetch(
        "https://my-chat-application-4cbad-default-rtdb.firebaseio.com/messsages.json"
      )
        .then((response) => {
          if (response.ok) {
            return response.json();
          }
        })
        .then((data) => {
          console.log(data);
          const messagesData = [];
          for (const id in data) {
            messagesData.push({
              id: id,
              user: data[id].name,
              text: data[id].msg,
            });
          }
          console.log(messagesData);
          // this.messages = this.messages.concat(messagesData);
          this.messages = messagesData;
        })
        .then(this.addMessage);
    },
  },
};
</script>


<style scoped>

.vertical-menu {  
  display: absolute;
  top: 30px;
  width: 800px;
  height: 400px;
  overflow-y: auto;
}
.message{
  background-color: #eee;
  color: black;
  display: block;
  padding: 12px;
  text-decoration: none;
}


.text-message {
  display: flex;
  position: sticky;
  width: 88%;
  position: absolute;
  text-align: left;
  bottom: 0px;
  height: 50px;
  padding: 5px;
  box-sizing: border-box;
}

.btn {
  position: sticky;
  width: 9%;
  position: absolute;
  top: 95%;
  left: 89%;
  height: 40px;
  background-color: #4caf50;
  border: none;
  color: white;
  padding: 15px 15px;
  text-align: center;
  text-decoration: none;
  display: block;
  font-size: 16px;
}

.scroll {
  margin: 4px, 4px;
  padding: 4px;
  background-color: #08c708;
  width: 300px;
  overflow: auto;
  white-space: nowrap;
}

.container {
  display: block;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  height: 400px;
  width: 800px;
  background: #f2f2f2;
  overflow: hidden;
  border-radius: 20px;
  cursor: pointer;
  box-shadow: 0 0 20px 8px #d0d0d0;
  overflow: scroll;
}

.container {
  padding: 50px;
  background-color: lightblue;
}
/* The following tag selector input use the different properties for the text filed. */

input[type="text"] {
  width: 89%;
  padding: 15px;
  margin: 5px 0 22px 0;
  display: flex;
  border: none;
  background: #f1f1f1;
}

input[type="text"]:focus {
  background-color: rgb(241, 217, 171);
  outline: none;
}
div {
  padding: 10px 0;
}

hr {
  border: 1px solid #f1f1f1;
  margin-bottom: 25px;
}
/* The following tag selector button use the different properties for the Button. */
.submit {
  background-color: #4caf50;
  color: white;
  padding: 16px 20px;
  border: none;
  cursor: pointer;
  margin: 8px 0;
  width: 100%;
  opacity: 0.9;
}
.reset {
  background-color: #eb2b2b;
  color: white;
  padding: 16px 20px;
  border: none;
  cursor: pointer;
  margin: 8px 0;
  width: 100%;
  opacity: 0.9;
}

button:hover {
  opacity: 1;
}
.message {
  text-align: left;
}
</style>

