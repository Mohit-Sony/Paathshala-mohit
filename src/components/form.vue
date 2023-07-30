<template>
    <div>
        <p class="font-bold text-3xl">Add New Employee</p>
        <p v-if="error" class=" text-xl p-4 bg-red-100 border-red-400 rounded my-3 border-1 text-red-500">Error in creating new element : {{error}}</p>
        <p v-if="iscreated" class="text-xl p-4 bg-green-100 border-green-400 rounded my-3 border-1 text-green-500">New element creation successfull please press refresh btn to see changes </p>

        <div class=" border rounded-md bg-slate-200 mx-auto p-10 ">
            <p>Name</p>
            <input type="text" placeholder="Enter name " v-model="formData.Name" class="border w-full p-3 my-2 rounded">

            <p>Location</p>
            <input type="text" placeholder="Mumbai" v-model="formData.Location" class="border w-full p-3 my-2 rounded">

            <p>Job Title</p>
            <input type="text" placeholder="Senior Developer" v-model="formData.JobTitle" class="border w-full p-3 my-2 rounded">

            <p>Rate</p>
            <input type="number" v-model="formData.Rate" class="border w-full p-3 my-2 rounded">
           
            <p>Comment</p>
            <input type="textarea" placeholder="Comment...."  class="border w-full p-3 pt-6 my-2 rounded">

            <div @click="addEmployee" class="p-4 rounded bg-blue-400 max-w-sm text-white w-28  cursor-pointer" >
                ADD
            </div>
        </div>

    </div>
</template>

<script setup>
import { ref } from 'vue'
import { API } from 'aws-amplify';
import {createEmployee} from "./../graphql/mutations"
let error = ref("");
let iscreated = ref(false)
const jobOptions = ["All Jobs" , "Full TIme" , "Half Time" , "Remote" , "In Office"]
const formData = ref({
    Name: '',
    Location:'',
    JobTitle :'',
    Rate:"",
})

async function addEmployee() {
    let data = await API.graphql({
          query:createEmployee ,
          variables: {input:{Name: formData.value.Name, Location: formData.value.Location, Rate : formData.value.Rate , JobTitle:formData.value.JobTitle}}
        });

    console.log("data: ",data);
    if(!data){
        error.value = "unable to create new element try using proper data input";
        iscreated.value = false;
    }else{
        error.value = null;
        iscreated.value=true;
    }

}

</script>

<style lang="scss" scoped>

</style>