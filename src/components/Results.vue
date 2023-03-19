<script setup lang="ts">
import { ref, onMounted } from 'vue'

const props = defineProps({
    maleMajor: Object,
    maleCollege: Object,
    femaleMajor: Object,
    femaleCollege: Object,
})
</script>


<template>            
    <h2 class="text-center p-2 mb-3">Results</h2>
    <h4 class="text-center">Colleges</h4>
    <div class="row justify-content-center">
        <div class="card col-md-4 col-sm-6 m-2">
            <ul class="p-0 list-group-flush ">
                <h2 class="text-center mb-3">Males</h2>
                <div v-for="(college, index) in maleCollege">
                    <hr class="m-2">
                    <li class="list-group-item d-flex justify-content-between align-items-center">
                        {{index + 1}} - {{college.name}}
                        <span class="badge color-accent">{{parseFloat(college.rating).toFixed(2)}}</span>
                    </li>
                </div>
                <hr class="m-2">
            </ul>
        </div>
        <div class="card col-md-4 col-sm-6 m-2">
            <ul class="p-0 list-group-flush ">
                <h2 class="text-center mb-3">Females</h2>
                <div v-for="(college, index) in femaleCollege">
                    <hr class="m-2">
                    <li class="list-group-item d-flex justify-content-between align-items-center">
                        {{index + 1}} - {{college.name}}
                        <span class="badge color-accent">{{parseFloat(college.rating).toFixed(2)}}</span>
                    </li>
                </div>
                <hr class="m-2">
            </ul>
        </div>
    </div>

    <h4 class="text-center mt-5">Majors</h4>
    <div class="row justify-content-center">
        <div class="card col-md-4 col-sm-6 m-2">
            <ul class="p-0 list-group-flush ">
                <h2 class="text-center mb-3">Males</h2>
                <div v-for="(major, index) in maleMajor">
                    <hr class="m-2">
                    <li class="list-group-item d-flex justify-content-between align-items-center">
                        {{index + 1}} - {{major.name}}
                        <span class="badge color-accent">{{parseFloat(major.rating).toFixed(2)}}</span>
                    </li>
                </div>
                <hr class="m-2">
            </ul>
        </div>
        <div class="card col-md-4 col-sm-6 m-2">
            <ul class="p-0 list-group-flush">
                <h2 class="text-center mb-3">Females</h2>
                <div v-for="(major, index) in femaleMajor">
                    <hr class="m-2">
                    <li class="list-group-item d-flex justify-content-between align-items-center">
                        {{index + 1}} - {{major.name}}
                        <span class="badge color-accent">{{parseFloat(major.rating).toFixed(2)}}</span>
                    </li>
                </div>
                <hr class="m-2">
            </ul>
        </div>
    </div>
</template>

<style scoped>
@import "../assets/base.css";

h2 {
    color: var(--color-heading);
}
.color-accent-male {
    color: var(--color-accent-male);
}
.color-accent-female {
    color: var(--color-accent-female);
}

.color-accent {
    color: var(--color-accent);
}

.card {
    padding: 10px;
    background-color: var(--color-background-soft);
    border-radius: 10px;
    /*box-shadow: 0 0 10px 0 var(--c-dark-2);*/
}
</style>
