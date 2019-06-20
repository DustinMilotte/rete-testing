<template>
  <div id="container">
    <header>
      <h2 class="title">
        <textarea
          v-model="selectedNode.name"
          name="node-title"
          id="node-title"
          cols="50"
          rows="1"
          placeholder="enter node title here"
        ></textarea>
      </h2>
      
    </header>
    <hr>
    <section id="main-section">
      <div class="voice-select">
        <h2 id="voice-label">Voice</h2>
        <div>
          <select v-model="selectedNode.data.voice">
            <option>Male</option>
            <option>Female</option>
          </select>
        </div>
      </div>
      <div id="main-text-container">
        <textarea
          v-model="selectedNode.data.dialogue"
          id="dialogue-textarea"
          cols="40"
          rows="20"
          placeholder="enter main dialogue here..."
        ></textarea>
      </div>
      <h2>Attach Audio Files</h2>
      <div id="file-drop-area">drop files here...</div>
    </section>
    <button class="btn-close" @click="$emit('close-form')">Save</button>
  </div>
</template>

<script>
export default {
  name: "AudNodeForm",
  methods: {
    saveAndClose() {
      //TODO save data to current node

      // close form
      // this.formIsShowing = false;
      console.log("save and close");
    }
  },
  props: ["selectedNode"]
};
</script>

<style scoped>
textarea {
  resize: none;
}
#container {
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  height: 80%;
  width: 40%;
  background: #eee;
  position: fixed;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  border: 1px solid black;
  padding: 2rem;
  border-radius: 10px;
}
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 10%;
}
#main-section {
  display: flex;
  flex-direction: column;
  height: 90%;
}
#node-title {
  width: 90%;
  margin: 0rem 10rem 0rem 1rem;
  padding-left: 1rem;
  font-size: 2rem;
}
.title {
  margin-bottom: 0;
}
#voice-label {
  display: inline;
  padding-right: 2rem;
}
.voice-select {
  display: flex;
  align-items: center;
}
.btn-close {
  margin: 0.5rem;
  border-radius: 10%;
  font-size: 1.5rem;
  color: #666;
  padding: .5rem;
  width: 100px;
  align-self: center;
}
.btn-close:hover {
  color: #333;
  align-self: center;
}

#main-text-container {
  height: 30vh;
}
#dialogue-textarea {
  height: 100%;
  width: 100%;
}

#file-drop-area {
  height: 100px;
  width: 80%;
  background: white;
  border: 1px solid grey;
}
select {
  display: inline;
}
</style>
