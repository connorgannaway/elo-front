<script setup lang="ts">
    import axios from "axios";
    import {onMounted, ref} from 'vue'


    import Spinner from "@/components/Spinner.vue";
    import Results from "@/components/Results.vue";

    const apipath = "http://127.0.0.1:8000"

    const chosenGender = ref(false)
    const gender = ref('')
    const count = ref(0)
    const remaining = ref(10)
    const major1 = ref()
    const major2 = ref()
    const maleList = ref()
    const femaleList = ref()
    const maleCollege = ref()
    const femaleCollege = ref()
    const loading = ref(false)
    const collegesLoaded = ref(false)
    const majorsLoaded = ref(false)
    

    function handleVote(m1win: boolean) {
        count.value++
        remaining.value--
        
        let promise = new Promise((resolve, reject) => {
            
            let data = {
                item1: major1.value.pk,
                item2: major2.value.pk,
                item1win: m1win,
                gender: gender.value
            }

            axios.post(apipath + '/api/votes/', data)
            .then(response => {
                console.log(response)
                resolve(response.status)
            })
            .catch(error => {
                console.log(error)
                alert(error.message + " occured whilst sending vote. Please try again later.")
                reject(error.status)
            })
        })


        promise.then(
            (successMessage) => {
                console.log(successMessage)
                refreshMajors()
            },
            (failureMessage) => {
                console.log(failureMessage)
                refreshMajors()
            }
        )

        if(remaining.value == 0){
            getAllMajors()
            getColleges()
        }
    }

    function chooseGender(g:string) {
        chosenGender.value = true
        gender.value = g
        //getAllMajors()
        refreshMajors()
    }

    function refreshMajors(){
        if(gender.value == 'M'){
            let result = getRandom(maleList.value, 2)
            major1.value = result[0]
            major2.value = result[1]
        } else {
            let result = getRandom(femaleList.value, 2)
            major1.value = result[0]
            major2.value = result[1]
        }

    }

    function getRandom(arr:object[], n:number) {
        var result = new Array(n),
            len = arr.length,
            taken = new Array(len);
        if (n > len)
            throw new RangeError("getRandom: more elements taken than available");
        while (n--) {
            var x = Math.floor(Math.random() * len);
            result[n] = arr[x in taken ? taken[x] : x];
            taken[x] = --len in taken ? taken[len] : len;
        }
        return result;
    }

    function getAllMajors(){
        loading.value = true
        let maleLoaded = false
        let femaleLoaded = false

        console.log("Getting all majors")
        axios.get(apipath + '/api/items/?gender=m')
        .then(response => {
            maleList.value = response.data
            console.log(response.data)
            maleLoaded = true
            if(maleLoaded && femaleLoaded){
                loading.value = false
            }
        })
        .catch(error => {
            console.log(error)
            alert(error.message + " occured whilst fetching majors. Refresh and/or try again later.")
        })

        axios.get(apipath + '/api/items/?gender=f')
        .then(response => {
            femaleList.value = response.data
            femaleLoaded = true
            if(maleLoaded && femaleLoaded){
                loading.value = false
            }
        })
        .catch(error => {
            console.log(error)
            alert(error.message + " occured whilst fetching majors. Refresh and/or try again later.")
        })

    }

    function getColleges(){
        axios.get(apipath + '/api/colleges/?gender=m')
        .then(response => {
            maleCollege.value = response.data
        })
        .catch(error => {
            console.log(error)
        })

        axios.get(apipath + '/api/colleges/?gender=f')
        .then(response => {
            femaleCollege.value = response.data
        })
        .catch(error => {
            console.log(error)
        })
    }

    function refreshLists(){
        getAllMajors()
        getColleges()
    }

    function calcProgress(step: number){
        if(count.value >= step){
            return "is-current"
        }
    }

    onMounted(() => {
        getAllMajors()
        getColleges()
    });
    


</script>

<template>


        <header class="text-center">
            <h1 class="display-2 mb-2">Best College Major?</h1>
            <p class="h5">The decision on the most popular college major.</p>
        </header>
    
        <div v-if="!chosenGender" class="">
            <h3 class="text-center">Vote for Male or Female?</h3>
            <div class="row justify-content-center">
                <button class="select col-md-4 col-sm-6" @click="() => chooseGender('M')">Male</button>
                <button class="select col-md-4 col-sm-6" @click="() => chooseGender('F')">Female</button>
            </div>
        </div>
        <div v-else>
            <h3 class="text-center">Which 
                <span v-if="gender=='M'" class="color-accent-m" @click="chosenGender = false" style="cursor: pointer"><u>Male</u></span>
                <span v-else class="color-accent-f" @click="chosenGender = false" style="cursor: pointer"><u>Female</u></span>
                Major is Better?
            </h3>
    
            <div v-if="loading" class="h-100 d-flex align-items-center justify-content-center">
                <Spinner />
            </div>
    
            <div v-else>
                <div class="row justify-content-center">
                    <button class="select  col-md-4 col-sm-6 " @click="() => handleVote(true)">{{major1.name}}</button>
                    <button class="select  col-md-4 col-sm-6" @click="() => handleVote(false)">{{major2.name}}</button>
                </div>
            </div>
        
            <div v-if="remaining > 0">
                <ol class="ProgressBar mt-3">
                    <li class="ProgressBar-step">
                        <svg class="ProgressBar-icon" :class="calcProgress(1)" ><use xlink:href="#checkmark-bold"/></svg>
                    </li>
                    <li class="ProgressBar-step">
                        <svg class="ProgressBar-icon" :class="calcProgress(2)" ><use xlink:href="#checkmark-bold"/></svg>
                    </li>
                    <li class="ProgressBar-step">
                        <svg class="ProgressBar-icon" :class="calcProgress(3)" ><use xlink:href="#checkmark-bold"/></svg>
                    </li>
                    <li class="ProgressBar-step">
                        <svg class="ProgressBar-icon" :class="calcProgress(4)" ><use xlink:href="#checkmark-bold"/></svg>
                    </li>
                    <li class="ProgressBar-step">
                        <svg class="ProgressBar-icon" :class="calcProgress(5)" ><use xlink:href="#checkmark-bold"/></svg>
                    </li>
                    <li class="ProgressBar-step">
                        <svg class="ProgressBar-icon" :class="calcProgress(6)" ><use xlink:href="#checkmark-bold"/></svg>
                    </li>
                    <li class="ProgressBar-step">
                        <svg class="ProgressBar-icon" :class="calcProgress(7)" ><use xlink:href="#checkmark-bold"/></svg>
                    </li>
                    <li class="ProgressBar-step">
                        <svg class="ProgressBar-icon" :class="calcProgress(8)" ><use xlink:href="#checkmark-bold"/></svg>
                    </li>
                    <li class="ProgressBar-step">
                        <svg class="ProgressBar-icon" :class="calcProgress(9)" ><use xlink:href="#checkmark-bold"/></svg>
                    </li>
                    <li class="ProgressBar-step">
                        <svg class="ProgressBar-icon" :class="calcProgress(10)" ><use xlink:href="#checkmark-bold"/></svg>
                    </li>
                </ol>
                <p class="text-center mt-0"><em>Results unlock after your ranking.</em></p>
            </div>
            

        <div v-if="remaining <= 0">
            
            <Results 
            :male-college="maleCollege" 
            :female-college="femaleCollege" 
            :male-major="maleList" 
            :female-major="femaleList" 
            />
        </div>
    </div>


</template>

<style scoped>
@import "../assets/base.css";


.text-center {
    text-align: center;
}

header {
    margin-bottom: 2rem;
    position: relative;

}
h1 {
    color: var(--color-heading);
}

.color-accent {
    color: var(--color-accent);
}
.color-accent-f{
    color: var(--color-accent-female);
}
.color-accent-m{
    color: var(--color-accent-male);
}

.color-accent-grad {
    font-weight: bold;
    background: linear-gradient(to bottom right, var(--color-accent), rgb(240, 125, 17));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;

}

.select {
    background-color: var(--color-background);
    color: var(--color-text);
    border: 2px;
    border-radius: 5px;
    border-style: solid;
    border-color: var(--color-accent);
    padding: 1rem;
    margin: 1rem;
    font-size: 1.5rem;
    transition-duration: 0.4s;
    margin-top: 1.5rem;
}

.select:hover {
    background-color: var(--color-background-mute);
    /*color: var(--color-background);*/
}



.ProgressBar {
    margin: 0 auto;
    padding: 2em 20em 2em 20em;
    list-style: none;
    position: relative;
    display: flex;
    justify-content: space-between;
}

@media screen and (max-width: 1024px) {
    .ProgressBar {
        padding: 2em 10em 2em 10em;
    }
}

@media screen and (max-width: 926px) {
    .ProgressBar {
        padding: 2em 4em 2em 4em;
    }
}

.ProgressBar-icon {
  width: 1em;
  height: 1em;
  background-color: var(--color-background-mute);
  fill: var(--color-background-mute);
  border-radius: 50%;
  padding: 0.5em;
  max-width: 100%;
  z-index: 10;
  position: relative;
  transition: all .25s ease-out;
}
  .ProgressBar-icon.is-current {
    fill: var(--color-accent);
    background-color: var(--color-accent);
  }
  


</style>