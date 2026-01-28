<template>
    <div class="container">
        
        <!-- <embed id="fred" style="border:1px solid #666CCC" title="PDF-file in an i-Frame" type='application/pdf-file' src="../assets/img5.jpg" frameborder="1" scrolling="auto" width="50%" height="600px" ></embed> -->
        <div class="noDocsCont" v-if="this.$parent.docsJSON.length < 1">no documents found</div>
        <div class="docsCont">
            <div class="docContainer" v-for="doc in this.$parent.docsJSON" :key="doc.ID">
                <div class="docDetailsContainer">
                    <div class="logoCont" :class="{'excel-file' : doc.file_url.split('.').pop() == 'xlsx' || doc.file_url.split('.').pop() == 'xls' , 'pdf-file' : doc.file_url.split('.').pop() == 'pdf', 'word-file' : doc.file_url.split('.').pop() == 'docx', 'word-file' : doc.file_url.split('.').pop() == 'doc'}"></div>
                    <div class="docTitle"> {{ doc.file_url ? doc.file_url.split('/').pop() : '' }} </div>
                </div>
                <div>
                    <div class="downloadBTN" @click="downloadDoc(doc)"></div>
                </div>
            </div>
        </div>
        <div class="newDocCont">
            <div class="docContainer addDoc">upload a new document</div>
        </div>
        <div class="docViewer">
        </div>
    </div>
</template>

<script>


export default {

  data() {
    return {
      
    }
  },
  methods:{
    handleClick(event){
        console.log(event.target)
    },
    getExtension(url){
        let ext = ''
        doc.file_url ? ext = doc.file_url.split('.').pop() : ext = ''

    },
    downloadDoc(doc){
        const headers = new Headers();
        const url = `http://172.18.1.202:8000/api/method/erpnext.api.get_all_files_url?folder_name='${doc.file_url}'`
        console.log(url);
        let token = 'Token eb3b867c1f34fdf:ee5aa1b03576bbb'
        headers.append('Authorization', token);
        headers.append('cache-control', 'no-cache');

        const init = {
            method: 'GET',
            headers
        }

        fetch(url, init)
        .then(response => {
            // console.log(response);
            return response.blob();
        }) // Get the response as a Blob
        .then(blob => {
            // Handle the file data (e.g., create a downloadable link in a browser)
            const url = window.URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.style.display = 'none';
            a.href = url;
            a.download = `${doc.file_url.split('/').pop()}`; // Set a desired filename
            document.body.appendChild(a);
            a.click();
            window.URL.revokeObjectURL(url);
        })
        .catch(error => console.error('Error fetching file:', error));
    }
  }
}
</script>

<style scoped>
    .container{
        /* margin: auto; */
        flex-direction: column;
        display: flex;
        position: relative;
        justify-content: space-between;
        align-items: start;
        padding-top: 50px;
        padding-bottom: 10px;
        transition: .3s;
        margin: 10px;
    }
    /* .docViewer{
        z-index: 2;
        display: absolute;
        width: 100%;
    } */
    .docsCont{
        height: 60vh;
        overflow-y: auto;
        width: 100%;
        padding-left: 10%;
        padding-right: 10%;
        box-sizing: border-box;
    }
    .docsCont::-webkit-scrollbar{
      width: 1em;
    }
    .docsCont::-webkit-scrollbar-track{
      background: rgb(0, 0, 0,.1);
      border-radius: 10px;
      margin-block: 10px;
    }
    .docsCont::-webkit-scrollbar-thumb{
      background: rgb(0, 0, 0,.1);
      border: 1px solid rgb(0, 0, 0, 0.05);
      cursor: pointer;
      border-radius: 10px;
    }
    .docsCont::-webkit-scrollbar-thumb:hover{
      background: rgb(0, 0, 0,.15);
    }
    .newDocCont{
        width: 100%;
        margin-top: 30px;
        padding-left: 15%;
        padding-right: 15%;
        box-sizing: border-box;
    }
    .noDocsCont{
        font-size: x-large;
        color: rgb(0,0,0,.5);
        margin: 50px;
    }
    .logoCont{
        color: rgb(240,240,240);
        height: 100%;
        padding: 13px;
        min-width: 100px;
        box-sizing: border-box;
        padding-left: 30px;
        padding-right: 30px;
        border-right: whitesmoke 2px solid;
    }
    .logoCont::before, .logoCont{
        font-family: "Font Awesome 6 Free"; /* Or "Font Awesome 6 Brands" */
        content: "\f15b"; /* Unicode for the user icon */
        padding-right: 15px;
        padding-top: 10px;
        color: rgb(50,50,100);
        background-color: #ccc;
    }
    .excel-file::before, .excel-file{
        font-family: "Font Awesome 6 Free"; /* Or "Font Awesome 6 Brands" */
        content: "\f1c3"; /* Unicode for the user icon */
        color: white;
        background-color: rgb(51, 196, 129);
    }
    .pdf-file::before, .pdf-file{
        font-family: "Font Awesome 6 Free"; /* Or "Font Awesome 6 Brands" */
        content: "\f1c1"; /* Unicode for the user icon */
        background-color: rgb(244, 15, 2);
        color: white;
        padding-right: 13px;
    }
    .word-file::before, .word-file{
        font-family: "Font Awesome 6 Free"; /* Or "Font Awesome 6 Brands" */
        content: "\f1c2"; /* Unicode for the user icon */
        background-color: rgb(0, 161, 241);
        /* background-color: rgb(0, 164, 239); */
        color: white;
    }
    .docTitle{
        margin: auto;
        margin-left: 30px;
        margin-right: 30px;
        padding-top: 10px;
        max-width: 50vw;
        height: 100%;
        /* width: auto; */
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
    }
    .docContainer{
        display: flex;
        justify-content: space-between;
        width: 100%;
        height: 40px;
        margin-bottom: 10px;
        border-radius: 8px;
        font-size: large;
        font-weight: 550;
        padding: auto;
        background-color: rgb(0,0,0,.1);
        border: 1px solid rgb(0,0,0,.1);
        box-sizing: border-box;
        /* cursor: pointer; */
        /* pointer-events: none; */
        transition: .3s ease-in-out;
        /* transition-delay: 1s; */
    }
    .docContainer:hover{
        background-color: rgb(0,0,0,.15);
        color: black;
        transform: scale(1.02);
        user-select: none;
        border-radius: 15px;
    }
    .addDoc{
        background-color: rgba(195, 225, 255, 0.5);
        color: rgb(0,0,0,.5);
        justify-content: center;
        align-items: center;
        text-align: center;
        border: 2px dashed gray;
        height: 75px;
        border-radius: 10px;
    }
    .addDoc:hover{
        background-color: rgba(195, 225, 255, 0.6);
    }
    .downloadBTN{
        /* align-self: center;
        align-items: center; */
        background-color: #ccc;
        color: rgb(50,50,100);
        border-left: 2px dashed rgb(50,50,100,.3);
        min-width: 100px;
        height: 100%;
        padding-top: 10px;
        margin: auto;
        /* margin-right: 50px; */

        /* pointer-events: all; */
        cursor: pointer;
        transition: .3s ease-in-out;
    }
    .downloadBTN:hover{
        border-left: 2px dashed rgb(25,25,75);
    }
    .downloadBTN::before{
        font-family: "Font Awesome 6 Free";
        content: "\f019";
        transition: .2s
    }
    .downloadBTN:hover::before{
        color: rgb(25,25,75);
    }
    .docDetailsContainer{
        display: flex;
    }
</style>