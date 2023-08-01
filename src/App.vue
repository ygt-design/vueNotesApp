<script setup>
  import { ref } from 'vue';

  const showModel = ref(false);
  const newNote = ref('');
  const errorMessage = ref('');
  const notes = ref([]);

  function getRandomColor() {
    return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
  }

  const addNote = () => {
    if(newNote.value.length < 10){
      return errorMessage.value = 'Note must be at least 10 characters long';
    }
    notes.value.push({
      id: Math.floor(Math.random() * 100000),
      text: newNote.value,
      date: new Date().toLocaleDateString(),
      backgroundColor: getRandomColor(),
    });
    showModel.value = false;
    newNote.value = '';
    errorMessage.value = '';
  }
</script>

<template> 
  <main>
    <div v-if="showModel" class="overlay">
      <div class="modal">
        <textarea v-model.trim="newNote" name="notes" id="note" cols="30" rows="10"></textarea>
        <p v-if="errorMessage"> {{ errorMessage }}</p>
        <button @click="addNote"> Add Notes </button>
        <button @click="showModel = false" class="close"> Close </button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1> Notes </h1>
        <button @click="showModel = true"> + </button>
      </header>
      <div class="card-container">
        <div 
          v-for="note in notes" 
          :key="note.id"
          class="card" 
          :style="{backgroundColor: note.backgroundColor}">
          <p class="main-text"> 
            {{ note.text }}
          </p>
          <p class="date">
            {{ note.date }}
          </p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
  main{
    height: 100vh;
    width: 100vw;
  }
  .container{
    max-width: 1000px;
    padding: 10px;
    margin: 0 auto;
  }

  header{
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  h1{
    font-weight: bold;
    margin-bottom: 25px;
    font-size: 75px;
  }

  header button{
    border: none;
    padding: 10px;
    widows: 50px;
    height: 50px;
    cursor: pointer;
    background-color: rgba(255, 255, 255, 0.65);
    border-radius: 100%;
    aspect-ratio: 1;
    color: black;
    font-size: 20px;
  }
  .card{
    width: 225px;
    aspect-ratio: 1;
    background-color: rgb(237, 182, 44);
    border-radius: 15px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    margin-right: 20px;
    margin-bottom: 20px;
    color: black;
    font-size: 15px;
    padding: 10px;
  }
  .date{
    font-size: 12.5px;
    font-weight: bold;
  }
  .card-container{
    display: flex;
    flex-wrap: wrap;
  }
  .overlay{
    position: absolute;
    height: 100vh;
    width: 100vw;
    background-color: rgba(0, 0, 0, 0.65);
    z-index: 1;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .modal{
    background-color: white;
    padding: 30px;
    border-radius: 15px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-between;
    position: relative;
  }
  .modal button{
    padding: 10px 20px;
    font-size: 20px;
    width: 100%;
    border: none;
    background-color: black;
    color: white;
    cursor: pointer;
    margin: 15px;
    border: 5px;
  }
  .modal .close{
    background-color: rgb(135, 135, 135);
    margin-top: 7px;
  }

  .modal p{
    color: black;
  }
</style>