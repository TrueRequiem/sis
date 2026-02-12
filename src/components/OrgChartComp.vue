<template>
    <div class="container" id="cont" ref="cont">
        <div class="controls">
            <div class="cont-btn" @click="recenter"><i class="fa-solid fa-arrow-rotate-right"></i></div>
            <div class="cont-btn" @click="enlarge"><i class="fa-solid fa-plus"></i></div>
            <div class="cont-btn" @click="minimize"><i class="fa-solid fa-minus"></i></div>
        </div>
        <!-- <div class="tree">
            <ul>
                <li><a><span>President</span><img class="employee"><span>Worker</span></a>
                <ul>
                    <li><a><span>Manager</span><img class="employee"><span>Sub Worker</span></a>
                    <ul>
                        <li> <a><span>Employee</span><img class="employee"><span>1st Sub Worker</span></a> </li>
                        <li> <a><span>Employee</span><img class="employee"><span>1st Sub Worker</span></a> </li>
                    </ul>
                </li>
                <li> <a @click="collapse($event)"><span>Manager</span><img class="employee"><span>Sub Worker</span></a>
                <ul>
                    <li> <a @click="collapse($event)"><span>Employee</span><img class="employee"><span>1st Sub Worker</span></a>
                        <ul>
                            <li> <a><span>Employee</span><img class="employee"><span>2nd Sub Worker</span></a> </li>
                            <li> <a><span>Employee</span><img class="employee"><span>2nd Sub Worker</span></a> </li>
                        </ul>
                    </li>
                    <li> <a><span>Employee</span><img class="employee"><span>1st Sub Worker</span></a> </li>
                    <li> <a @click="collapse($event)"><span>Employee</span><img class="employee"><span>1st Sub Worker</span></a>
                        <ul>
                            <li> <a><span>Employee</span><img class="employee"><span>2nd Sub Worker</span></a> </li>
                            <li> <a><span>Employee</span><img class="employee"><span>2nd Sub Worker</span></a> </li>
                            <li> <a><span>Employee</span><img class="employee"><span>2nd Sub Worker</span></a> </li>
                        </ul>
                    </li>
                </ul>
            </li>
            <li><a><span>Manager</span><img class="employee"><span>Sub Worker</span></a></li>
        </ul>
    </li>
            </ul>
        </div> -->

        <div v-if="this.$parent.chartsIsLoaded" name="tree" class="tree" ref="tree" @dblclick="handleClick($event); collapse($event); " @contextmenu.prevent="collapse($event)" v-html="orgChartHTML"></div>
    </div>
</template>

<script>
import { ref } from 'vue';
    export default {
        components: {
        },

        data(){
            return{
                treeScale : 1,
                // loaded: true,
            }
        },

        setup(){
            
            let orgChartHTML = ref(['<ul><li><a><img class="company-sekem"><span>SEKEM</span></a><ul><ul><li><a><span>President</span><img class="employee"><span>Worker</span></a><ul><li><a><span>Manager</span><img class="employee"><span>Sub Worker</span></a><ul><li> <a><span>Employee</span><img class="employee"><span>1st Sub Worker</span></a> </li><li> <a><span>Employee</span><img class="employee"><span>1st Sub Worker</span></a> </li></ul></li><li> <a><span>Manager</span><img class="employee"><span>Sub Worker</span></a><ul><li> <a><span>Employee</span><img class="employee"><span>1st Sub Worker</span></a><ul><li> <a><span>Employee</span><img class="employee"><span>2nd Sub Worker</span></a> </li><li> <a><span>Employee</span><img class="employee"><span>2nd Sub Worker</span></a> </li></ul></li><li> <a><span>Employee</span><img class="employee"><span>1st Sub Worker</span></a> </li><li> <a><span>Employee</span><img class="employee"><span>1st Sub Worker</span></a><ul><li> <a><span>Employee</span><img class="employee"><span>2nd Sub Worker</span></a> </li><li> <a><span>Employee</span><img class="employee"><span>2nd Sub Worker</span></a> </li><li> <a><span>Employee</span><img class="employee"><span>2nd Sub Worker</span></a> </li></ul></li></ul></li><li><a><span>Manager</span><img class="employee"><span>Sub Worker</span></a></li></ul></li></ul>']);
            let chartJSONReq = ref([]);
            let treeLoaded = ref([true]);
            let currentDepInd = '';
            let companyName = 'SEKEM';

            fetch('http://172.18.1.202:8000/api/method/erpnext.api.getDepartment?company_name="OrabiHospital"')
            .then(response => response.json())
            .then(data => {
                chartJSONReq = data.message;
                orgChartHTML.value = chartInit(chartJSONReq);
            })
            .catch(error => console.log(error))
            
            const chartInit = (orgChart) =>{
                
                return `<ul><li><a><img class="${'company-sekem'}"><span>${companyName}</span></a>` + createChartOne(orgChart)
            }
            const createChartOne = (orgChart) =>{
            let html = '<ul>'
            for (let i = 0; i < orgChart.length; i++) {
                html += orgChart[i].grade == '4-Leadership (Directors)' ? `<li><a class="director">` : orgChart[i].grade == '3-Senior-Level-(Managerial)' ? `<li><a class="manager">` : (orgChart[i].grade == '2-Mid-Level-(Profissional)' || orgChart[i].grade == '1- Entry-level  (Junior)')  ? `<li><a class="employee">`  : '<li><a>';
                html += orgChart[i].designation != null ? `<span type='${orgChart[i].name}'>${orgChart[i].designation}</span><img class="employee">` : '';
                html += orgChart[i].is_group == 1 ? `<img class="department">` : '';
                html += orgChart[i].is_group == 0 ? `<img class="sub">` : '';
                html += orgChart[i].employee_name != null ? `<span type='${orgChart[i].reports_to}'>${orgChart[i].employee_name}</span></a>` : '';
                html += orgChart[i].employee_name == null ? `<span>${orgChart[i].name}</span></a>` : '';
                html += orgChart[i].children != null ? createChartOne(orgChart[i].children) : '';
                html += '</li>';
            }
            html += '</ul>';
            return html;  
            }

            const injectChildren = (parent, child, id) =>{
                // console.log(parent)
                for (let i = 0; i < parent.length; i++) {
                    if(parent[i].name.toUpperCase() == id){
                        parent[i].children = child;
                        currentDepInd = i;
                    }
                    else(parent[i].children = null)
                }
                return parent
            }

             const injectEmployee = (parent, child, id) =>{
                for (let i = 0; i < parent[currentDepInd].children.length; i++) {
                    if(parent[currentDepInd].children[i].name.toUpperCase() == id){
                        parent[currentDepInd].children[i].children = child;
                    }
                }
                return parent
            }

            const filterGrade = (json) =>{
                let highestGrade = 1
                if(json.length > 0){
                for(let i = 0; i < json.length; i++){
                    if(json[i].grade[0] > highestGrade){
                        highestGrade = json[i].grade[0];}
                }
                switch (highestGrade) {
                    case '4':
                        highestGrade = '4-Leadership (Directors)'
                        break;
                    case '3':
                        highestGrade = '3-Senior-Level-(Managerial)'
                        break;
                    case '2':
                        highestGrade = '2-Mid-Level-(Profissional)'
                        break;
                    default:
                        highestGrade = '1- Entry-level  (Junior)'
                    }
                    }
                if(highestGrade == '3-Senior-Level-(Managerial)' || highestGrade == '4-Leadership (Directors)'){
                const filteredGrade = json.filter(index => {
                return index.grade === highestGrade;});
                return filteredGrade}
                else{ return json}
            }

            const parseJSON = (json, child, objID) =>{
                for(let i in json){
                    if(json[i].name == objID){
                        json[i].children = (child)
                    }else{
                        if(json[i].children)
                            parseJSON(json[i].children, child, objID)
                    }
                    
                }
            } 

            const handleClick = (event) =>{
                // this.$parent.chartIsLoaded = false;
                if(event.target.tagName == 'A'){
                    if(event.target.firstElementChild != null && event.target.nextElementSibling == null){
                        if(event.target.firstElementChild.classList.contains("department")){
                            treeLoaded.value = false;
                            fetch(`http://172.18.1.202:8000/api/method/erpnext.api.getSubDepartment?main_dept="${event.target.innerText}"`)
                            .then(response => response.json())
                            .then(data => {
                                // console.log(data.message);
                                treeLoaded.value = true;
                                if(data.message.length > 0){
                                    orgChartHTML.value = chartInit(injectChildren(chartJSONReq, data.message, event.target.innerText));
                                
                                    event.target.scrollIntoView({
                                    behavior: 'smooth', // Animates the scroll smoothly
                                    block: 'start',      // Aligns the top of the element with the top of the viewport (default)
                                    inline: 'center' 
                                    });

                                }else{
                                        // event.target.style.opacity = .7;
                                        // event.target.style.scale = .9;
                                        event.target.style.transform  = 'rotateY(360deg)';
                                    }
                                // else{orgChartHTML.value += '<div></div>'}
                            }).catch(error => {
                                console.log(error);
                                treeLoaded.value = true;
                            });

                        }
                        if(event.target.firstElementChild.classList.contains("sub")){
                            // console.log(event.target.innerText)
                            treeLoaded.value = false;
                            fetch(`http://172.18.1.202:8000/api/method/erpnext.api.getEmpfrmDept?emp_dept="${event.target.innerText}"`)
                            .then(response => response.json())
                            .then(data => {
                                treeLoaded.value = true;
                                if(data.message.length > 0){
                                    parseJSON(chartJSONReq, data.message, event.target.innerText);
                                    orgChartHTML.value = chartInit(injectEmployee(chartJSONReq, filterGrade(data.message), event.target.innerText));

                                    event.target.scrollIntoView({
                                    behavior: 'smooth', // Animates the scroll smoothly
                                    block: 'start',      // Aligns the top of the element with the top of the viewport (default)
                                    inline: 'center' 
                                    });
                                }else{
                                        // event.target.style.opacity = .7;
                                        // event.target.style.scale = .9;
                                        event.target.style.transform  = 'rotateY(360deg)';
                                    }
                                // else{orgChartHTML.value += '<div></div>'}
                        }).catch(error => {
                                console.log(error);
                                treeLoaded.value = true;
                            });

                        }

                        if(event.target.firstElementChild.nextElementSibling.classList.contains("employee") ){
                            
                            if(event.target.firstElementChild.getAttribute('type') != 'null'){
                                // console.log(event.target.firstElementChild.getAttribute('type'))
                                treeLoaded.value = false;
                                fetch(`http://172.18.1.202:8000/api/method/erpnext.api.get_my_employee?manager_id="${event.target.firstElementChild.getAttribute('type')}"`)
                                .then(response => response.json())
                                .then(data => {
                                    treeLoaded.value = true;
                                    if(data.message.length > 0){
                                        parseJSON(chartJSONReq, data.message, event.target.firstElementChild.getAttribute('type'));
                                        orgChartHTML.value = chartInit(chartJSONReq, filterGrade(data.message), event.target.innerText);

                                        event.target.firstElementChild.nextElementSibling.nextElementSibling.scrollIntoView({
                                        behavior: 'smooth', // Animates the scroll smoothly
                                        block: 'start',      // Aligns the top of the element with the top of the viewport (default)
                                        inline: 'center' 
                                        });
                                    }else{
                                        // event.target.style.opacity = .7;
                                        // event.target.style.scale = .9;
                                        event.target.style.transform  = '';
                                        event.target.style.transform  = 'rotateY(360deg)';
                                    }
                                    // else{orgChartHTML.value += '<div></div>'}
                                    // console.log(data.message);
                            }).catch(error => {
                                console.log(error);
                                treeLoaded.value = true;
                            });      

                            } 

                        }
                    }
                }
                // console.log(event.target.firstElementChild)
                // console.log(event.target.firstElementChild.nextElementSibling)
                
            }

            return{
                orgChartHTML,
                chartJSONReq,
                treeLoaded,
                chartInit,
                createChartOne,
                injectChildren,
                injectEmployee,
                handleClick,
            }

            
        },
        methods:{
            collapse(event){
                if(event.target.tagName == 'A'){
                let temp = 'nothing'
                let temp1 = 'nothing'
                if(event.target.nextElementSibling != null){
                    temp = event.target.nextElementSibling.style.display;
                    temp1 = event.target.nextElementSibling.tagName;
                    setTimeout(() => {
                    this.$parent.chartIsLoaded = true;
                    }, 200);
                }
                if(temp == "" && temp1 == 'UL'){
                    event.target.nextElementSibling.style.opacity = "0";
                    setTimeout(function() {
                        event.target.nextElementSibling.style.display = "none";  
                    }, 300);
                    // event.target.style.backgroundColor = "wheat";
                    // event.target.style.opacity = .7;
                    // event.target.style.scale = 1
                    
                }
                else{
                    if(temp1 == 'UL'){
                    event.target.nextElementSibling.style.display = "";
                    setTimeout(function() {
                        event.target.nextElementSibling.style.opacity = "1";
                    }, 100);
                    // event.target.style.backgroundColor = "";
                    // event.target.style.opacity = "1";
                    // event.target.style.scale = ''
                    };
                }
                }
            },
            enlarge(){
                if(this.treeScale < 1.5){
                this.$refs.tree.style.scale = this.treeScale + 0.2
                this.treeScale += 0.2
                this.$refs.tree.style.paddingTop = '150px'
                this.$refs.tree.style.paddingBottom = ''

                }
            },
            minimize(){
                if(this.treeScale > .6){
                    this.$refs.tree.style.scale = this.treeScale - 0.2
                    this.treeScale -= 0.2
                    this.$refs.tree.style.paddingTop = ''
                    this.$refs.tree.style.paddingBottom = 0
                }
            },
            recenter(){
                this.$refs.tree.style.scale = ''
                this.treeScale = 1
                this.$refs.tree.style.paddingTop = ''
                this.$refs.tree.style.paddingBottom = ''
                setTimeout(() => 
                {this.$refs.tree.scrollIntoView({
                behavior: 'smooth', // Animates the scroll smoothly
                block: 'start',      // Aligns the top of the element with the top of the viewport (default)
                inline: 'center' 
                })}, 250)
            },
            // handleClick(event){
            //     this.$parent.chartIsLoaded = false;
            //     if(event.target.tagName == 'A'){
            //         if(event.target.firstElementChild != null && event.target.nextElementSibling == null){
            //             if(event.target.firstElementChild.classList.contains("department")){
                        
            //                 fetch(`http://172.18.1.202:8000/api/method/erpnext.api.getSubDepartment?main_dept="${event.target.innerText}"`)
            //                 .then(response => response.json())
            //                 .then(data => {
            //                     // console.log(data.message);
            //                     if(data.message.length > 0)
            //                     console.log(this.orgChartHTML)
            //                         this.orgChartHTML = this.chartInit(this.injectChildren(this.chartJSONReq, data.message, event.target.innerText));
            //                 });

            //                 event.target.scrollIntoView({
            //                 behavior: 'smooth', // Animates the scroll smoothly
            //                 block: 'end',      // Aligns the top of the element with the top of the viewport (default)
            //                 inline: 'center' 
            //                 });

            //             }
            //             if(event.target.firstElementChild.classList.contains("sub")){
                        
            //                 fetch(`http://172.18.1.202:8000/api/method/erpnext.api.getEmpfrmDept?emp_dept="${event.target.innerText}"`)
            //                 .then(response => response.json())
            //                 .then(data => {
            //                     if(data.message.length > 0){
            //                     parseJSON(chartJSONReq, data.message, event.target.innerText);
            //                     orgChartHTML.value = createChartOne(injectEmployee(chartJSONReq, filterGrade(data.message), event.target.innerText));
            //                     }
            //             });
            //                 event.target.scrollIntoView({
            //                 behavior: 'smooth', // Animates the scroll smoothly
            //                 block: 'end',      // Aligns the top of the element with the top of the viewport (default)
            //                 inline: 'center' 
            //                 });

            //             }

            //             if(event.target.firstElementChild.nextElementSibling.classList.contains("employee") ){
                            
            //                 if(event.target.firstElementChild.getAttribute('type') != 'null'){
            //                     console.log(event.target.firstElementChild.getAttribute('type'))
            //                     fetch(`http://172.18.1.202:8000/api/method/erpnext.api.get_my_employee?manager_id="${event.target.firstElementChild.getAttribute('type')}"`)
            //                     .then(response => response.json())
            //                     .then(data => {
            //                         if(data.message.length > 0){
            //                             parseJSON(chartJSONReq, data.message, event.target.firstElementChild.getAttribute('type'));
            //                             orgChartHTML.value = createChartOne(chartJSONReq, filterGrade(data.message), event.target.innerText);

            //                             setTimeout(event.target.firstElementChild.nextElementSibling.nextElementSibling.scrollIntoView({
            //                             behavior: 'smooth', // Animates the scroll smoothly
            //                             block: 'start',      // Aligns the top of the element with the top of the viewport (default)
            //                             inline: 'center' 
            //                             }), 100)
                                        
            //                         }
            //                         // console.log(data.message);
            //                 })      

            //                 } 

            //             }
            //         }
            //     }
                
            // }
        },
        watch: {
            treeLoaded(value){
                if(value == true)
                    setTimeout(() => {this.$parent.chartIsLoaded = true}, 250)
                    // emit('chartIsLoading', false);
                if(value == false)
                    this.$parent.chartIsLoaded = false;
            }
        },
        mounted(){

            setTimeout(() => 
                {this.$refs.tree.scrollIntoView({
                behavior: 'smooth', // Animates the scroll smoothly
                block: 'start',      // Aligns the top of the element with the top of the viewport (default)
                inline: 'center' 
                })}, 200)
            
        },
    }
</script>

<style scoped>
    .container{
        /* display: hidden; */
        position: relative;
        justify-content: center;
        align-items: start;
        transition: .3s;
        transition-behavior: allow-discrete;
    }
    .controls{
        z-index: 1;
        left: 50px;
        top: 15px;
        width: min-content;
        position: sticky;
        border-radius: 50px;
        padding: 20px;
        display: flex;
        justify-content: space-between;
        /* background-color: rgb(230,235,255); */
        /* box-shadow: rgba(0, 0, 0, 0.05) 0px 0px 0px 1px; */
    }
    .cont-btn{
        background-color: rgb(220,220,255);
        border: 1px solid rgb();
        box-shadow: rgba(0, 0, 0, 0.35) 0px 3px 7px;
        border-radius: 15px;
        box-sizing: border-box;
        padding: 15px;
        margin-right: 20px;
        transition: .3s;
    }
    .cont-btn:hover{
        background-color: rgb(210,210,255);
        scale: 1.1;
        /* rotate: 5deg; */
    }
</style>

<style>

.tree {
    padding-top: 20px;
    padding-bottom: 150px;
	width: 1000%;
	height: auto;
	text-align: center;
    user-select: none; 
    transition: .2s ease-in-out;
    transition-behavior: allow-discrete;
}
.tree ul {
    /* max-width: 2000px; */
	padding-top: 20px;
	position: relative;
	transition: .5s;
}
.tree li {
	display: inline-table;
	text-align: center;
	list-style-type: none;
	position: relative;
	padding: 10px;
	transition: .5s;
}
.tree li ul, .tree ul{
    overflow: hidden;
}
.tree li::before, .tree li::after {
	content: '';
	position: absolute;
	top: 0;
	right: 50%;
	border-top: 2px solid #ccc;
    z-index: -1;
	width: 51%;
	height: 10px;
    transition: .5s;
}
.tree li::after {
    z-index: -1;
	right: auto;
	left: 50%;
	border-left: 2px solid #ccc;
}
.tree li:only-child::after, .tree li:only-child::before {
	display: none;
}
.tree li:only-child {
	padding-top: 3px;
}
.tree li:first-child::before, .tree li:last-child::after {
	border: 0 none;
}
.tree li:last-child::before {
	border-right: 2px solid #ccc;
    z-index: -1;
	border-radius: 0 5px 0 0;
	-webkit-border-radius: 0 5px 0 0;
	-moz-border-radius: 0 5px 0 0;
}
.tree li:first-child::after {
	border-radius: 5px 0 0 0;
	-webkit-border-radius: 5px 0 0 0;
	-moz-border-radius: 5px 0 0 0;
}
.tree ul ul::before {
	content: '';
	position: absolute;
	top: 0;
	left: 50%;
    z-index: -1;
	border-left: 2px solid #ccc;
	width: 0;
	height: 20px;
}
.tree li a {
    background-color: rgb(0,100,0,.3);
    min-width: 100px;
	border: 1px solid #ccc;
	padding: 10px;
	display: inline-grid;
	border-radius: 5px;
	text-decoration-line: none;
	border-radius: 5px;
	transition: .5s;
    cursor: pointer;
    /* border-bottom-right-radius: 100px; */
}
.closeTreeNode{
    display: none;
}
.tree li a.director{
    background-color: rgb(200,0,0,.3);
}
.tree li a.manager{
    background-color: rgb(0,0,100,.3);
}
.tree li a.employee{
    background-color: rgb(255,200,0,.3);
}
.tree li a img{
    background-color: white;
	width: 55px;
	height: 55px;
	margin-bottom: 10px !important;
    margin-top: 10px !important;
    border-radius: 100px;
	margin: auto;
    pointer-events: none;
}
img.employee{
    content: url("../assets/user.jpg");
}
img.department{
    content: url("../assets/building.png");
}
img.sub{
    content: url("../assets/sub.png");
}
img.company-sekem{
    content: url("../assets/CompanyLogos/Sekem\ Holding\ Logo.png");
    object-fit:contain;
    padding: 5px;
    box-sizing: border-box;
    border-radius: 20px !important; 
}
img.company-isis{
    content: url("../assets/CompanyLogos/Isis\ Logo.png");
    object-fit:contain;
    padding: 5px;
    box-sizing: border-box;
    border-radius: 20px !important; 
}
img.company-atos{
    content: url("../assets/CompanyLogos/ATOS\ Logo.png");
    object-fit:contain;
    padding: 5px;
    box-sizing: border-box;
    border-radius: 20px !important; 
}
img.company-lotus{
    content: url("../assets/CompanyLogos/LOTUS\ Logo.png");
    object-fit:contain;
    padding: 5px;
    box-sizing: border-box;
    border-radius: 20px !important; 
}
img.company-nature{
    content: url("../assets/CompanyLogos/NatureTex\ Logo.png");
    object-fit:contain;
    padding: 5px;
    box-sizing: border-box;
    border-radius: 20px !important; 
}
.extraDetails{
    margin-top: -10px;
    
}
.extraDetails > span{
    color: white !important;
    background-color: lightgreen !important;

}
.extraDetails > span ~ span{
    color: white !important;
    background-color: lightcoral !important;
}
.tree li a span {
	border: 1px solid #aaa;
    background-color: white;
	border-radius: 5px;
	color: #444;
    max-width: 100px;
	padding: 5px;
	font-size: 10px;
	text-transform: uppercase;
	letter-spacing: 1px;
	font-weight: 500;
    overflow: hidden; 
    text-overflow: ellipsis; 
    pointer-events: none;
}
.tree li a span.reports{
    /* max-height: 0px; */
    display: none;
    overflow: hide;
    transition: .2s;
    /* box-sizing: border-box; */
}

/*Hover-Section*/
.tree li a:hover, .tree li a:hover i, .tree li a:hover , .tree li a:hover+ul li a {
	background: #c8e4f8;
	color: #000;
	border: 1px solid #94a0b4;
    scale: 1.05;
}
.tree li a:hover, .tree li a:hover i, .tree li a:hover span, .tree li a:hover+ul li a {
	color: black;
}
.tree li a:hover+ul li::after, .tree li a:hover+ul li::before, .tree li a:hover+ul::before, .tree li a:hover+ul ul::before {
	border-color: #94a0b4;
}
.tree li a:hover > .reports{
    display: block;
    /* margin-top: 2px; */
    margin-top: 5px;

}

</style>


    // const headers = new Headers();
    // const url = "http://172.18.1.202:8000/"
    
    // let token = 'token b39b0fdd52bd092:a1ed9cb2d9267a8'
    // headers.append('Authorization', token);
    // headers.append('cache-control', 'no-cache');

    // const init = {
    //     method: 'GET',
    //     headers
    // }

    // fetch(url, init)
    // .then((response) => {
    //     return response.json(); // or .json() or .blob() ...
    // })
    // .then((response) => {
    //     // this.responseData = response.message;
    //     // console.log(response) // text is the response body
    //     console.log(response.message) 
    // })
    // .catch((e) => {
    //     console.log(e) // error in e.message
    // });