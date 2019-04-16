<template>
  <div id="container">
    <div class="send">
      <p><b>Send CV:</b></p>
      <input type="text" v-model="name" placeholder="name">
      <input type="text" v-model="surname" placeholder="surname">
      <input type="number" min="0" max="100" v-model="age" placeholder="age">
      <select v-model="position">
        <option disabled value="">Please select position</option>
        <option>Front-End</option>
        <option>Back-End</option>
        <option>Dev-Ops</option>
        <option>Project Manager</option>
        <option>UI/UX</option>
      </select>
      <textarea v-model="experience" placeholder="experience" rows="5"></textarea>
      <button class='button' @click="sendCv">Send CV</button>
    </div>

    <div>
      <p><b>List of sent CVs:</b></p>
      <p v-if="cvJSON.elements == 0">You do not have any CVs yet</p>
      <div v-for="(element, index) in cvJSON.elements" class='elements'>
        {{index + 1}}.
        <div class='element'>
          <select v-model="cvJSON.elements[index].position" disabled>
            <option disabled value="">Please select position</option>
            <option>Front-End</option>
            <option>Back-End</option>
            <option>Dev-Ops</option>
            <option>Project Manager</option>
            <option>UI/UX</option>
          </select>
          <input type="text" v-model="cvJSON.elements[index].name" placeholder="name" disabled>
          <input type="text" v-model="cvJSON.elements[index].surname" placeholder="surname" disabled>
          <input type="number" min="0" max="100" v-model="cvJSON.elements[index].age" placeholder="age" disabled>
          <input type="text" v-model="cvJSON.elements[index].experience" placeholder="experience" disabled>
        </div>
        <button class='button editButton' @click="updateCv(index)">Edit CV</button>
        <button class='button' @click="deletedCv(index)">Delete CV</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      name: null,
      surname: null,
      age: null,
      position: null,
      experience: null,
      cvJSON: {
        elements: [],
      },
    }
  },
  methods: {
    sendCv() {
      this.cvJSON.elements.push({
        "name": this.name,
        "surname": this.surname,
        "age": this.age,
        "position": this.position,
        "experience": this.experience,
      });

      this.setLocalStorage();
      this.name = null;
      this.surname = null;
      this.age = null;
      this.position = null;
      this.experience = null;
    },

    updateCv(index) {
      const elements = document.getElementsByClassName('element')[index].childNodes;
      let editButton = document.getElementsByClassName('editButton')[index];

      if (editButton.textContent != 'Save') {
        for (let i = 0; i < elements.length; i++) {
          elements[i].disabled = false;
        }
        editButton.textContent = 'Save';
      } else {
        this.setLocalStorage();
        for (let i = 0; i < elements.length; i++) {
          elements[i].disabled = true;
        }
        editButton.textContent = 'Edit CV';
      }
    },
    deletedCv(index) {
      this.cvJSON.elements.splice(index,1);
      this.setLocalStorage();
    },
    setLocalStorage() {
      localStorage.setItem('cvJSON', JSON.stringify(this.cvJSON));
    },
  },
  mounted() {
    let localData = JSON.parse(localStorage.getItem('cvJSON'))
    if (localData != null) {
      this.cvJSON = localData;
    };
  },
};
</script>

<style scoped lang="scss">
#container {
  .button {
    background: rgb(128, 128, 128);
    color: white;
    border-radius: 5px;
    height: 30px;
    &:hover{
      background: rgba(128, 128, 128, 0.8);
      cursor: pointer;
    }
  }
  display: flex;
  flex-direction: column;
  align-items: center;
  font-family: "Lucida Sans Unicode", "Lucida Grande", sans-serif;
  .send {
    margin: 15px;
    border: 2px solid rgb(128, 128, 128);
    border-radius: 5px;
    background: rgba(134, 121, 121, 0.2);
    * {
      display: block;
      margin: 5px;
      width: 200px;
      border-radius: 5px;
    }
  }
  .elements {
    border: 2px solid rgb(128, 128, 128);
    background: rgba(134, 121, 121, 0.2);
    border-radius: 5px;
    padding: 5px;
    margin: 5px;
  }
}
</style>