<template>
    <div class="container">
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

        <div class="tree" @dblclick="handleClick($event); collapse($event)" @contextmenu.prevent="collapse($event)" v-html="orgChartHTML"></div>
    </div>
</template>

<script>
import ListHierarchy from './ListHierarchyComp.vue';
import orgChartJSON from '../json/orgChart.json';
import { ref } from 'vue';
    export default {
        components: {
            ListHierarchy
        },

        data(){
            return{
                result : ref([]),
            }
        },

        setup(){
            let orgChartHTML = ref('<ul><li><a><span>President</span><img class="employee"><span>Worker</span></a><ul><li><a><span>Manager</span><img class="employee"><span>Sub Worker</span></a><ul><li> <a><span>Employee</span><img class="employee"><span>1st Sub Worker</span></a> </li><li> <a><span>Employee</span><img class="employee"><span>1st Sub Worker</span></a> </li></ul></li><li> <a><span>Manager</span><img class="employee"><span>Sub Worker</span></a><ul><li> <a><span>Employee</span><img class="employee"><span>1st Sub Worker</span></a><ul><li> <a><span>Employee</span><img class="employee"><span>2nd Sub Worker</span></a> </li><li> <a><span>Employee</span><img class="employee"><span>2nd Sub Worker</span></a> </li></ul></li><li> <a><span>Employee</span><img class="employee"><span>1st Sub Worker</span></a> </li><li> <a><span>Employee</span><img class="employee"><span>1st Sub Worker</span></a><ul><li> <a><span>Employee</span><img class="employee"><span>2nd Sub Worker</span></a> </li><li> <a><span>Employee</span><img class="employee"><span>2nd Sub Worker</span></a> </li><li> <a><span>Employee</span><img class="employee"><span>2nd Sub Worker</span></a> </li></ul></li></ul></li><li><a><span>Manager</span><img class="employee"><span>Sub Worker</span></a></li></ul></li></ul>');
            let chartJSONReq = ref([]);
            let currentDepInd = ref([]);

            // fetch('http://172.18.1.202:8000/api/method/erpnext.api.getDepartment?company_name="OrabiHospital"')
            // .then(response => response.json())
            // .then(data => {
            //     chartJSONReq = data.message;
            //     orgChartHTML.value = createChartOne(chartJSONReq);
            // })

            // fetch('https://api.example.com/data')
            // .then(response => {
            //     if (!response.ok) {
            //     // Manually throw an error
            //     throw new Error(`HTTP error! Status: ${response.status}`);
            //     }
            //     return response.json();
            // })
            // .then(data => console.log(data))
            // .catch(error => {
            //     // This block catches the thrown error from the .then() block
            //     // as well as any network errors
            //     console.error("Fetch error:", error.message);
            // });
            
                        
            const createChartOne = (orgChart) =>{
            let html = '<ul>';
            for (let i = 0; i < orgChart.length; i++) {
                html += `<li><a>`
                html += orgChart[i].designation != null ? `<span>${orgChart[i].designation}</span><img class="employee">` : '';
                html += orgChart[i].is_group == 1 ? `<img class="department">` : '';
                html += orgChart[i].is_group == 0 ? `<img class="sub">` : '';
                html += orgChart[i].employee_name != null ? `<span>${orgChart[i].employee_name}</span></a><ul class="extraDetails"><li><a><span>${orgChart[i].grade}</span><br><span>${orgChart[i].gender}</span></li></ul>` : '';
                html += orgChart[i].employee_name == null ? `<span>${orgChart[i].name}</span></a>` : '';
                html += orgChart[i].children != null ? createChartOne(orgChart[i].children) : '';
                html += '</li>';
            }
            html += '</ul>';
            return html;  
            }

            const injectChildren = (parent, child, id) =>{
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

            

            const handleClick = (event) =>{

                this.$parent.chartIsLoaded = false;

                console.log(event.target.innerText)
                fetch(`http://172.18.1.202:8000/api/method/erpnext.api.getSubDepartment?main_dept="${event.target.innerText}"`)
                .then(response => response.json())
                .then(data => {
                    // console.log(data.message);
                    if(data.message.length > 0)
                        orgChartHTML.value = createChartOne(injectChildren(chartJSONReq, data.message, event.target.innerText));
                this.$parent.chartIsLoaded = true;
            });

                fetch(`http://172.18.1.202:8000/api/method/erpnext.api.getEmpfrmDept?emp_dept="${event.target.innerText}"`)
                .then(response => response.json())
                .then(data => {
                    console.log(data.message);
                    if(data.message.length > 0)
                        // console.log(injectEmployee(chartJSONReq, data.message, event.target.innerText))
                        orgChartHTML.value = createChartOne(injectEmployee(chartJSONReq, data.message, event.target.innerText));
                this.$parent.chartIsLoaded = true;
            })
                
            }


            // orgChartHTML = createChartOne(orgChartJSON);
            // orgChartHTML = createChartTwo(chartJSONReq);
            

            return{
                orgChartHTML,
                handleClick,
            }

            
        },
        methods:{
            collapse(event){
                this.$parent.chartIsLoaded = false;
                let temp = 'nothing'
                let temp1 = 'nothing'
                if(event.target.nextElementSibling != null){
                    temp = event.target.nextElementSibling.style.display;
                    temp1 = event.target.nextElementSibling.tagName;
                }
                if(temp == "" && temp1 == 'UL'){
                    event.target.nextElementSibling.style.opacity = "0";
                    setTimeout(function() {
                        event.target.nextElementSibling.style.display = "none";
                    }, 400);
                    event.target.style.backgroundColor = "wheat";
                }
                else{
                    if(temp1 == 'UL'){
                    event.target.nextElementSibling.style.display = "";
                    event.target.style.backgroundColor = "red";
                    setTimeout(function() {
                        event.target.nextElementSibling.style.opacity = "1";
                    }, 100);
                    event.target.style.backgroundColor = "";
                    };
                }
                setTimeout(() => {
                    this.$parent.chartIsLoaded = true;
                    }, 750);
            },
        },
        mounted(){

        // fetch('http://172.18.1.202:8000/api/method/erpnext.api.getDepartment')
        // .then(response => response.json())
        // .then(data => console.log(data.message));

        // const headers = new Headers();
        // const url = "http://172.18.1.202:8000/api/method/erpnext.api.getDepartment"
        
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
        // this.$refs.companiesDropdown.innerHTML = "companies"

        // console.log(this.$refs.companies);
        // fetch('http://172.18.1.202:8000/api/method/library_management.library_management.doctype.library_member.api.get_all_company')
        // .then(response => response.json())
        // .then(data => console.log(data.message));

        }
    }
</script>

<style scoped>
    .container{
        /* margin: auto; */
        position: relative;
        justify-content: center;
        align-items: start;
        padding: 10px;
        transition: .3s;
        margin: 10px;
        padding-top: 20px;
    }
</style>

<style>

.tree {
	width: 100%;
	height: auto;
	text-align: center;
    user-select: none; 
}
.tree ul {
	padding-top: 20px;
	position: relative;
	transition: .5s;
}
.tree li {
	display: inline-table;
	text-align: center;
	list-style-type: none;
	position: relative;
    /* background-color: ; */
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
	padding-top: 0;
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
    background-color: rgb(0,0,0,.07);
	border: 1px solid #ccc;
	padding: 10px;
	display: inline-grid;
	border-radius: 5px;
	text-decoration-line: none;
	border-radius: 5px;
	transition: .5s;
    cursor: pointer;
    
}
.tree li a img{
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

</style>