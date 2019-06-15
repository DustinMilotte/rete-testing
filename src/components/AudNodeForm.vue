<template>
    <div id="container">
       <header>
           <h2 class="title">
               <textarea name="node-title" id="node-title" cols="50" rows="1" placeholder="enter node title here"></textarea>
           </h2>
           <button class="btn-close" v-on:click ="saveAndClose">X</button>
       </header>
       <hr>
        <section id="main-section">
            <h2>Text</h2>
            <textarea name="main-text-area" id="" cols="40" rows="20" placeholder="enter main dialogue here..."></textarea>
            <h2>Attach Audio Files</h2>
            <div id="file-drop-area">drop files here...</div>
        </section>
    </div>
</template>

<script>
export default {
    name: "AudNodeForm",
    methods:{
        saveAndClose(){
            //TODO save data to current node

            // close form
            document.getElementById('container').style.display = "none";
        }
    }
}

function saveAndClose(){
    alert("hello");
}
</script>

<style scoped>
    textarea{
        resize: none;
    }
    #container{
        height:80%;
        width:40%;
        background: #eee;
        position: fixed;
        left: 50%;
        top: 50%;
        transform: translate(-50%, -50%);
        border: 1px solid black;
        padding: 1rem;
        border-radius: 10px;
    }
    header {
        display: flex;
        justify-content: space-between;
    }
    #node-title{
        width: 90%;
        margin: 0rem 10rem 0rem 1rem ;
        padding:0;
        font-size: 2rem;
    }
    .title {
        margin-bottom: 0;
    }
    .btn-close {
        margin: .5rem;
    }
    #main-section {
        display:flex;
        flex-direction: column;
    }
    #file-drop-area {
        height: 100px;
        width: 80%;
        background: white;
        border: 1px solid grey;
    }
</style>
