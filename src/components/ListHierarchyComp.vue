<template>
    <div class="listCont"><ul v-html="listHTML" @click="handleClick($event)" @contextmenu.prevent="handleOptions($event)"></ul></div>
</template>

<style scoped>

    *{
        text-align: left;
        user-select: none;
    }
    .listCont{
        transform-origin: left;
        padding-top: 10px;
        padding-bottom: 10px;
        padding-right: 30px;
        height: 100%;
        box-sizing: border-box;
        overflow-y: auto;
        border-radius: 5px;
    }
    .listCont::-webkit-scrollbar{
      width: .5em;
    }
    .listCont::-webkit-scrollbar-track{
      background: rgb(0, 0, 0,.1);
      border-radius: 10px;
      margin-block: 20px;
    }
    .listCont::-webkit-scrollbar-thumb{
      background: rgb(0, 0, 0,.1);
      border: 1px solid rgb(0, 0, 0, 0.05);
      cursor: pointer;
      border-radius: 10px;
    }
    .listCont::-webkit-scrollbar-thumb:hover{
      background: rgb(0, 0, 0,.15);
    }
    /* .listCont::-webkit-scrollbar{
        display: none;
    } */
</style>

<style>
    li span{
        cursor: pointer;
    }
    ul{
        list-style-type: none;
        color: #666;
        margin-bottom: 10px;
    }
    ul span.folder{
        color: #555;
        font: 1.1rem sans-serif;
        text-transform: capitalize;
        cursor: pointer;
    }

    ul span.folder:hover,
    ul span.folder:hover::before,
    ul span.folder:hover + ul.nested> li,
    ul span.folder:hover + ul.nested li::after,
    ul span.folder:hover + ul.nested ul.nested li::before,
    ul span.folder:hover + ul.nested::before{
        color: crimson;
        border-color: crimson;
        transition: .4s ease-in-out;
        animation: LightenAnim 1.5s infinite linear;
    }

    ul.nested {
        position: relative;
        max-height: 0;
        max-width: 0;
        opacity: 0;
        overflow: hidden;
        transition: .5s ease-in-out;
    }

    ul.drop{
        position: relative;
        max-height: 500vh;
        max-width: 500vw;
        opacity: 1;
        overflow: hidden;
        transition: .5s ease-in-out;
    }

    ul.nested::before{
        position: absolute;
        content: "";
        left: 1rem;
        width: 1.5rem;
        height: 100%;
        border-left: 2px solid rgb(0,0,0,.5);
        transition: .2s ease-in-out ;
        border-radius: 2px;
    }

    

    ul.nested li{
        position: relative;
        padding-left: 3rem;
        transition: .2s ease-in-out ;
    }

    ul.nested li::after{
        position: absolute;
        content: "";
        left: 1rem;
        top: .32rem;
        width: 1.5rem;
        height: 1rem;
        border-bottom: 2px solid rgb(0,0,0,.5);
        border-radius: 2px;
        transition: .2s ease-in-out ;

    }

    ul span.folder::before,
    li.file-doc::before,
    li.file-cert::before,
    li.file-img::before{
        display: inline-block;
        margin-right: .4rem;
        font: 1.5rem sans-serif;
        transition: .2s ease-in-out ;
    }

    ul span.folder::before{
        content: '🗀';
        color: green;
        font-weight: 1000;
    }

    ul span.open::before{
        content: '🗁';
        color: green;
        padding-right: 5px;
        rotate: -5deg;
        font-weight: 1000;
        margin-left: 2px;
        scale: 1.1;
    }

    li.file-doc::before{
        content: '🗏';
        color: rgb(255, 150, 0, .6);
        cursor: pointer;
        font-weight: 1000;
    }

    li.file-cert::before{
        content: '🗎';
        color: rgb(50, 50, 200, .8);
        cursor: pointer;
        font-weight: 1000;
    }

    li.file-img::before{
        content: '⛼';
        color: rgb(0, 100, 100, .7);
        font-weight: 1000;
        cursor: pointer;
    }
    li.file-img:hover, li.file-cert:hover, li.file-doc:hover{
        padding-left: 3.5rem;
        font-size: 1.1rem;
        color: rgb(50,50,50);
    }
    li.file-doc:hover::before{
        animation: DarkenAnim .5s infinite linear;
    }
    li.file-img:hover::before, li.file-cert:hover::before{
        animation: DarkenAnim2 .5s infinite linear;
    }

    @keyframes DarkenAnim{
		100%,0%{
            filter: brightness(1);
		}
		25%{
            filter: brightness(0.9);
		}
		50%{
            filter: brightness(0.8);
		}
		75%{
            filter: brightness(.85);
		}
    }
    @keyframes DarkenAnim2{
		100%,0%{
            filter: brightness(1);
		}
		25%{
            filter: brightness(0.8);
		}
		50%{
            filter: brightness(0.6);
		}
		75%{
            filter: brightness(0.4);
		}
    }
    @keyframes LightenAnim{
		100%,0%{
            filter: brightness(.9);
		}
		25%{
            filter: brightness(1.1);
		}
		50%{
            filter: brightness(.9);
		}
		75%{
            filter: brightness(1.1);
		}
    }
</style>

<script>
import listJSON from '../json/list.json'
import heir from '../json/hierarchy.json'
import { ref } from 'vue';

    export default{

        data(){
            return{
                
            }
        },
        setup(){
            let listHTML = ref('<li> <span class="folder">Documents</span> <ul class="nested"> <li> <span class="folder">SEKEM</span> <ul class="nested"> <li class="file-doc"><span>Documents</span></li> <li class="file-cert"><span>Certificates</span></li> </ul> </li> <li> <span class="folder">ISIS</span> <ul class="nested"> <li class="file-doc"><span>Documents</span></li> <li class="file-cert"><span>Certificates</span></li> </ul> </li> <li> <span class="folder">Naturetex</span> <ul class="nested"> <li class="file-doc"><span>Documents</span></li> <li class="file-cert"><span>Certificates</span></li> </ul> </li> <li> <span class="folder">Lotus</span> <ul class="nested"> <li class="file-doc"><span>Documents</span></li> <li class="file-cert"><span>Certificates</span></li> </ul> </li> <li> <span class="folder">folders</span> <ul class="nested"> <li class="file-doc">something.xslx</li> <li class="file-cert">whatever.pdf</li> <li> <span class="folder">folders</span> <ul class="nested"> <li class="file-doc">something.xslx</li> <li class="file-cert">whatever.pdf</li> <li> <span class="folder">folders</span> <ul class="nested"> <li class="file-doc">something.xslx</li> <li class="file-cert">whatever.pdf</li> <li> <span class="folder">folders</span> <ul class="nested"> <li class="file-doc">something.xslx</li> <li class="file-cert">whatever.pdf</li> <li> <span class="folder">folders</span> <ul class="nested"> <li class="file-doc">something.xslx</li> <li class="file-cert">whatever.pdf</li>  </ul> </li> </ul> </li>  </ul> </li> </ul> </li> </ul> </li> </ul> </li> </ul> <ul> <li> <span class="folder">Organisation Charts</span> <ul class="nested"> <li> <span class="folder">SEKEM</span> <ul class="nested"> <li class="file-img"><span>SEKEM departments</span></li> <li class="file-img"><span>SEKEM Lab</span></li> </ul> </li> <li> <span class="folder">ISIS</span> <ul class="nested"> <li class="file-img"><span>ISIS Overview</span></li> <li class="file-img"><span>ISIS Factory</span></li> </ul> </li> <li> <span class="folder">Naturetex</span> <ul class="nested"> <li class="file-img"><span>Naturetex Overview</span></li> <li class="file-img"><span>Naturetex Factory</span></li> </ul> </li> <li> <span class="folder">Lotus</span> <ul class="nested"> <li class="file-img"><span>Lotus Overview</span></li> <li class="file-img"><span>Lotus Factory</span></li> </ul> </li> <li> <span class="folder">Lotus UE</span> <ul class="nested"> <li class="file-img"><span>Lotus UE Overview</span></li> <li class="file-img"><span>Lotus UE Factory</span></li> </ul> </li> </ul> </li>');

            const createList = (listTree) => {
                let html = '';
                for (let i = 0; i < listTree.length; i++) {
                    html += listTree[i].type == 'folder' ? `<li><span class="folder">${listTree[i].title}</span><ul class="nested">` : '';
                    html += listTree[i].type == 'doc' ? `<li class="file-doc" type="${listTree[i].src}"><span>${listTree[i].title}</span></li>` : '';
                    html += listTree[i].type == 'cert' || listTree[i].type == 'file' ? `<li class="file-cert" type="${listTree[i].src}"><span>${listTree[i].title}</span></li>` : '';
                    html += listTree[i].type == 'chart' ? `<li class="file-img" type="${listTree[i].src}"><span>${listTree[i].title}</span></li>` : '';
                    html += listTree[i].children != null ? createList(listTree[i].children) : '';
                    html += listTree[i].type == 'folder' ? `</li></ul>` : '';
                }
                return html;
            };


            listHTML.value = createList(listJSON)
            listHTML.value = createList(heir)

            return{
                listHTML
            }
        },
        methods:{
            handleClick(event){
                if(event.target.classList.contains('folder'))
                    this.expand(event)
                if(event.target.classList.contains('file-doc') || event.target.parentElement.classList.contains('file-doc') || event.target.classList.contains('file-cert') || event.target.parentElement.classList.contains('file-cert')){
                    this.showDocs();
                    let url = ''
                    event.target.type != undefined ? url = event.target.type : url = event.target.parentElement.type;
                    this.getDocs(url);
                    
                }
                    
                    
                if(event.target.classList.contains('file-img') || event.target.parentElement.classList.contains('file-img') )
                    {
                        if(event.target.parentElement.type && event.target.parentElement.type != undefined){
                            console.log(event.target.parentElement.type);}
                        if(event.target.type && event.target.type != undefined){
                            console.log(event.target.type);}
                        this.showChart();
                    }
            },
            handleOptions(event){
                if(event.target.classList.contains('file-cert') || event.target.classList.contains('file-doc')){
                    if( event.target.type != undefined){
                        console.log(event.target.type)}
                };
                if(event.target.parentElement.classList.contains('file-cert') || event.target.parentElement.classList.contains('file-doc')){
                    if( event.target.parentElement.type != undefined){
                        console.log(event.target.parentElement.type)}
                }
            },
            
            expand(event){
                if(event.target.nextElementSibling.classList == ("nested")){
                    // event.target.nextElementSibling.style.maxHeight = "500vh";
                    // event.target.nextElementSibling.style.maxWidth = "500vw";
                    // event.target.nextElementSibling.style.opacity = "1";
                    event.target.nextElementSibling.classList.add("drop")
                    event.target.classList.add("open");
                    event.target.style.color = 'rgb(50, 50, 50)';
                    event.target.style.fontSize = '1.25rem';
                }
                else{
                    // event.target.nextElementSibling.style.maxHeight = null;
                    // event.target.nextElementSibling.style.maxWidth = null;
                    // event.target.nextElementSibling.style.opacity = "0";
                    event.target.nextElementSibling.classList.remove("drop")
                    event.target.classList.remove("open");
                    event.target.style.color = '';
                    event.target.style.fontSize = '';
                }
            },
            showChart(){
                this.$parent.reset();
                this.$parent.showChart = true;
                this.$parent.chartsIsLoaded = false;
                setTimeout(() => {
                    this.$parent.chartsIsLoaded = true;
                    }, 1000);
            },
            showDocs(){
                this.$parent.reset();
                this.$parent.showDocs = true;
                this.$parent.docsIsLoaded = false;

                setTimeout(() => {
                    this.$parent.docsIsLoaded = true;
                    }, 1000);
            },
            getDocs(docURL){
                this.$parent.docsIsLoaded = false;
                const headers = new Headers();
                const url = `http://172.18.1.202:8000/api/method/erpnext.api.get_all_files_url?folder_name=${docURL}`
                console.log(url);
                let token = 'Token eb3b867c1f34fdf:ee5aa1b03576bbb'
                headers.append('Authorization', token);
                headers.append('cache-control', 'no-cache');

                const init = {
                    method: 'GET',
                    headers
                }

                fetch(url, init)
                .then((response) => {
                    return response.json(); // or .json() or .blob() ...
                }).then((response) => {
                    console.log(response);
                    this.$parent.docsJSON = response.message;
                    this.$parent.docsIsLoaded = true;
                })
            }
        },
    }
</script>


<!-- <div class="listCont" @click="handleClick($event)">
        <ul>
            <li>
                <span class="folder">Documents</span>
                <ul class="nested">
                    <li>
                    <span class="folder">SEKEM</span>
                    <ul class="nested">
                        <li class="file-doc"><span>Documents</span></li>
                        <li class="file-cert"><span>Certificates</span></li>
                    </ul>
                    </li>
                    <li>
                    <span class="folder">ISIS</span>
                    <ul class="nested">
                        <li class="file-doc"><span>Documents</span></li>
                        <li class="file-cert"><span>Certificates</span></li>
                    </ul>
                    </li>
                    <li>
                    <span class="folder">Naturetex</span>
                    <ul class="nested">
                        <li class="file-doc"><span>Documents</span></li>
                        <li class="file-cert"><span>Certificates</span></li>
                    </ul>
                    </li>
                    <li>
                    <span class="folder">Lotus</span>
                    <ul class="nested">
                        <li class="file-doc"><span>Documents</span></li>
                        <li class="file-cert"><span>Certificates</span></li>
                    </ul>
                    </li>
                    <li>
                    <span class="folder">folders</span>
                    <ul class="nested">
                        <li class="file-doc">something.xslx</li>
                        <li class="file-cert">whatever.pdf</li>
                        <li>
                    <span class="folder">folders</span>
                    <ul class="nested">
                        <li class="file-doc">something.xslx</li>
                        <li class="file-cert">whatever.pdf</li>
                        <li>
                    <span class="folder">folders</span>
                    <ul class="nested">
                        <li class="file-doc">something.xslx</li>
                        <li class="file-cert">whatever.pdf</li>
                        <li>
                    <span class="folder">folders</span>
                    <ul class="nested">
                        <li class="file-doc">something.xslx</li>
                        <li class="file-cert">whatever.pdf</li>
                        <li>
                    <span class="folder">folders</span>
                    <ul class="nested">
                        <li class="file-doc">something.xslx</li>
                        <li class="file-cert">whatever.pdf</li>
                    </ul>
                    </li>
                    </ul>
                    </li>
                    </ul>
                    </li>
                    </ul>
                    </li>
                    </ul>
                    </li>
                </ul>
            </li>
        </ul>

        <br>

        <ul>
            <li>
                <span class="folder">Organisation Charts</span>
                <ul class="nested">
                    <li>
                    <span class="folder">SEKEM</span>
                    <ul class="nested">
                        <li class="file-img"><span>SEKEM Holding</span></li>
                        <li class="file-img"><span>SEKEM Lab</span></li>
                    </ul>
                    </li>
                    <li>
                    <span class="folder">ISIS</span>
                    <ul class="nested">
                        <li class="file-img"><span>ISIS Overview</span></li>
                        <li class="file-img"><span>ISIS Factory</span></li>
                    </ul>
                    </li>
                    <li>
                    <span class="folder">Naturetex</span>
                    <ul class="nested">
                        <li class="file-img"><span>Naturetex Overview</span></li>
                        <li class="file-img"><span>Naturetex Factory</span></li>
                    </ul>
                    </li>
                    <li>
                    <span class="folder">Lotus</span>
                    <ul class="nested">
                        <li class="file-img"><span>Lotus Overview</span></li>
                        <li class="file-img"><span>Lotus Factory</span></li>
                    </ul>
                    </li>
                    <li>
                    <span class="folder">Lotus UE</span>
                    <ul class="nested">
                        <li class="file-img"><span>Lotus UE Overview</span></li>
                        <li class="file-img"><span>Lotus UE Factory</span></li>
                    </ul>
                    </li>
                </ul>
            </li>
        </ul>
    </div> -->