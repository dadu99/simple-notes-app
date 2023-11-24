<script setup>
  import {ref} from "vue";

  const showModal = ref(false);
  const newNote = ref("");
  const notes = ref([]);
  const errorMessage = ref("");

  function getRandomColor() {
    return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
  }

  const addNote = () => {
    if(newNote.value.length < 10) {
      return errorMessage.value = "Note need to be 10 characters or more."
    }

     notes.value.push({
        id: Math.floor(Math.random() * 1000000),
        text: newNote.value,
        date: new Date(),
        backgroundColor: getRandomColor()
    });
    showModal.value = false;
    errorMessage.value = "";
    newNote.value = "";
    
    console.log("notes array =>", notes.value);
  }

</script>

<template>
  <main>
     <div v-if="showModal" class="overlay">
      <div class="modal">
        {{ newNote  }}
        <button class="close" @click="showModal = false">&times;</button>
        <textarea v-model.trim="newNote" name="note" id="note" cols="30" rows="10"></textarea>
        <p v-if="errorMessage">{{ errorMessage }}</p>
        <button @click="addNote">Add Note</button>
      </div>
    </div> 
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal = true">+</button>
      </header>
      <div class="cards-container">
        <div class="card" 
          v-for="note in notes" :key="note.id" 
          :style="{backgroundColor: note.backgroundColor}">
          <p class="main-text">{{note.text}}</p>
          <p class="date">{{note.date.toLocaleDateString("en-US")}}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
  main {

    height: 100vh;
    width: 100vw;
    background-color: #ccc;
  }

  .container {
    max-width: 1000px;
    padding: 10px;
    margin: 0 auto
  }

  header {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  h1 {
    font-weight: bold;
    margin-bottom: 25px;
    font-size: 75px;
    color: black!important;;
  }

  header button {
    border: none;
    padding: 10px;
    width: 50px;
    height: 50px;
    cursor: pointer;
    background-color: rgb(232, 190, 53);
    border-radius: 100%;
    color: white;
    font-size: 20px;
  }

  .card {
    width: 225px;
    height: 225px;
    background-color: rgb(237, 182, 44);
    padding: 10px;
    border-radius: 15px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    margin-right: 20px;
    margin-bottom: 20px;
  }

  .date {
    font-size: 12.5px;
    font-weight: bold;
  }

  .cards-container {
    display: flex;
    flex-wrap: wrap;
  }

  .overlay {
    position: absolute;
    width: 100%;
    height: 100%;
    background-color: rgba(0,0,0,0.77);
    z-index: 10;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .modal {
    width: 750px;
    background: rgb(61,181,251);
    background: -moz-linear-gradient(150deg, rgba(61,181,251,1) 10%, rgba(178,125,202,1) 59%);
    background: -webkit-linear-gradient(150deg, rgba(61,181,251,1) 10%, rgba(178,125,202,1) 59%);
    background: linear-gradient(150deg, rgba(61,181,251,1) 10%, rgba(178,125,202,1) 59%);
    filter: progid:DXImageTransform.Microsoft.gradient(startColorstr="#3db5fb",endColorstr="#b27dca",GradientType=1);
    border-radius: 10px;
    padding: 30px;
    position: relative;
    display: flex;
    flex-direction: column;
    border: none;
  }
   .modal textarea {
    border: none;
    border-radius: 8px;
   }

  .modal button {
    padding: 10px 20px;
    font-size: 20px;
    width: 100%;
    background-color: #e238e7;
    border: none;
    color: white;
    cursor: pointer;
    margin-top: 15px;
    border-radius: 8px;
  }

  .modal .close {
    background-color: #ccc;
    margin-top: 7px;
    border-radius: 70%;
    display: flex;
    justify-content: center;
    position: relative;
    height: 40px;
    width: 40px;
    bottom: 33px;
    left: 668px;
  }
  .modal p {
    color: rgb(193, 15, 15);
  }

</style>