<template>
<!-- <div class="BTNsCont">
    <div class="cancelFilterBTNsCont">
        <div class="cancelBTN resetBTN" @click="resetList = []; flag = false">reset</div>
        <div  v-if="resetList.length > 0" style="display: flex;">
            <div v-for="index in resetList" class="cancelBTN" @click="removeFromList(index)">{{ index }}</div>
        </div>
    </div>
    <div class="DropdownCont" v-if="!flag">
        <div class="DropdownTitle" @click="flag = true" :class="{'hide' : resetList.length == 4 || flag}">{{ resetList.length < 1? "Companies" : resetList.length == 1? "Department" : resetList.length == 2? "Sub-Department" : resetList.length == 3? "Designation" : 1 }}</div>
        <div class="DropDownItemsCont" v-if="resetList.length < 1" ref="Dropdown">
            <div class="DropDownItems" v-for="index in compList" @click="this.addToList(index)">{{ index }}</div>
        </div>
        <div class="DropDownItemsCont" v-if="resetList.length == 1" ref="Dropdown">
            <div class="DropDownItems" v-for="index in depList" @click="this.addToList(index)">{{ index }}</div>
        </div>
        <div class="DropDownItemsCont" v-if="resetList.length == 2" ref="Dropdown">
            <div class="DropDownItems" v-for="index in SubdepList" @click="this.addToList(index)">{{ index }}</div>
        </div>
        <div class="DropDownItemsCont" v-if="resetList.length == 3" ref="Dropdown">
            <div class="DropDownItems" v-for="index in Designation" @click="this.addToList(index)">{{ index }}</div>
        </div>
    </div>
</div> -->
</template>

<style scoped>

.hide{
        scale: 0;
    }

.BTNsCont{
        display: flex;
        width: 90vw;
        justify-content: space-between;
    }
    .cancelFilterBTNsCont{
        min-height: 20px;
        width: 50%;
        display: flex;
        overflow: hidden;
        /* background-color: rgb(0,0,0,.01); */
        
    }
    .cancelBTN{
        position: relative;
        height: 0;
        align-items: center;
        cursor: default;
        margin-right: 10px;
        box-sizing: border-box;
        border-radius: 50px;
        background-color: rgb(0,0,0,.1);
        border: 2px solid rgb(0,0,0,.1);
        transition: .3s;
    }
    .cancelBTN:hover{
        scale: 1.2;
        rotate: 2deg;
    }
    .resetBTN{
        border: 2px solid red;
        margin-top: 1px;
        color: red;
        font-weight: 550;
        cursor: pointer;
        margin-right: 1rem;
    }
    .DropdownTitle{
        float: right;
        background-color: rgb(240,240,240);
        padding: 1rem  2rem 1rem 2rem;
        font-size: larger;
        max-width: 150px;
        font-weight: 550;
        border-radius: 10px;
        transition: .5s;
        cursor: pointer;
        position: relative;
    }
    .DropdownTitle:hover ~ .DropDownItemsCont, .DropDownItemsCont:hover{
        scale: 1;
    }
    .DropDownCont{
        position: relative;
    }
    .DropDownItemsCont{
        margin-top: 60px;
        padding: 10px;
        min-width: 200px;
        min-height: 100px;
        border-radius: 10px;
        scale: 0;
        background-color: rgb(240,240,240);
        box-shadow: rgba(99, 99, 99, 0.2) 0px 2px 8px 0px;
        transition: .5s;
    }
    .DropDownItems{
        cursor: pointer;
        margin: 10px;
        padding: 10px;
        border: 1px solid rgb(0,0,0,0);
        border-left: 1px solid rgb(0,0,0,.2);
        border-right: 1px solid rgb(0,0,0,.2);
        transition: .3s;
    }
    .DropDownItems:hover{
        font-weight: 550;
        scale: 1.1;
        border: 1px solid rgb(0,0,0,0);
        border-bottom: 1px solid rgb(0,0,0,.2);
        border-top: 1px solid rgb(0,0,0,.2);
        background-color: rgb(250,250,250);
        box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
    }
    .DropDownItems:nth-child(odd):hover{
        rotate: -1deg;
    }
    .DropDownItems:nth-child(even):hover{
        rotate: 1deg;
    }

</style>

<script>

    let compOption = '<option>Companies</option>';
    let flag = ref(true);
    let resetList = ref([]);
    let compList = ref(["NatureTex", "SEKEM", "SEKEM Holding", "SEKEM Lab", "ISIS", "Lotus"]);
    let depList = ref(["Accounting", "Financial", "Management", "Media"])
    let SubdepList = ref(["Sub-Dep1", "Sub-Dep2", "Sub-Dep3", "Sub-Dep4"])
    let Designation = ref(["First", "Second", "Third", "Fourth"])
    let dropDownTitle = ref('Companies');

    const addToList = (item) => {
        resetList.value.push(item);
        if(resetList.length == 1){
            dropDownTitle.value = 'Departments';
        }
        if(resetList.length == 2){
            dropDownTitle.value = 'Departments';
        }
    }

    const removeFromList = (item) =>{
        const index1 = resetList.value.indexOf(item);
        resetList.value.splice(index1 + 1);
    }
</script>