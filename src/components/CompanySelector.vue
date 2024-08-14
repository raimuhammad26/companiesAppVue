<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';

const companies = ref([]);

const selectedCompany = ref('');
const selectedTopic = ref('');
const showAnswers = ref(false);

const tamData = ref([]);


onMounted(async () => {
    try {
        const response = await axios.get('http://127.0.0.1:5000/company_list');
        companies.value = response.data.companies;
        selectedCompany.value = companies.value[0].name;
    } catch (error) {
        console.error('Error fetching company data:', error);
    }
});

onMounted(async () => {
    try {
        const response = await axios.get('http://127.0.0.1:5000/company_data');
        tamData.value = response.data.data; // Access the "data" array from the JSON response
    } catch (error) {
        console.error('Error fetching TAM data:', error);
    }
});

const selectTopic = (company, topic) => {
    selectedCompany.value = company;
    selectedTopic.value = topic;
    showAnswers.value = true;
};

const goBack = () => {
    showAnswers.value = false;
    selectedCompany.value = '';
    selectedTopic.value = '';
    selectedCompany.value = companies.value[0].name;
};

</script>

<template>

    <!-- Company Selector -->
    <div v-if="!showAnswers" class="com-selector">
        <label for="companies">Choose a company:</label>

        <select name="companies" id="companies" v-model="selectedCompany">
            <option v-for="company in companies" :key="company.name" :value="company.name">
                {{ company.name }}
            </option>
        </select>

        <div class="com-area">
            <div>Area 1</div>
            <button @click="selectTopic(selectedCompany, 'Topic 1')">Topic 1</button>
            <button @click="selectTopic(selectedCompany, 'Topic 2')">Topic 2</button>
        </div>

        <div style="display: flex;">
            <div>Area 2</div>
            <button @click="selectTopic(selectedCompany, 'Topic 3')">Topic 3</button>
        </div>
    </div>

    <!-- Company Answers -->
    <div v-if="showAnswers" class="com-answers">
        <div class="com-answers-header">
            <div class="com-ans-head-left">
                <h2>{{ selectedCompany }}</h2>
                <h2>{{ selectedTopic }}</h2>
            </div>

            <div>
                <button @click="goBack">Back</button>
            </div>
        </div>

        <div class="com-question">
            <h2>What is the TAM of the company?</h2>
        </div>

        <div class="com-answers-table">
            <table class="com-table">
                <thead>
                    <tr>
                        <th>#</th>
                        <th>TAM</th>
                        <th>Source</th>
                        <th>Date</th>
                        <th>Type</th>
                        <th>Select</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(item, index) in tamData" :key="index">
                        <td>{{ index + 1 }}</td>
                        <td>{{ item.TAM }}</td>
                        <td>{{ item.Source }}</td>
                        <td>{{ item.Date }}</td>
                        <td></td> <!-- Empty for now -->
                        <td></td> <!-- Empty for now -->
                    </tr>
                </tbody>
            </table>
        </div>
    </div>


</template>

<style scoped>
.com-selector{
    font-size: 17px;
    width: 500px;
    margin: 0 auto;
    margin-top: 30px;
    /* background-color: aqua; */
}

.com-selector button, .com-answers button{
    background: #252122;
    color: #fff;
    margin: 3px;
    margin-left: 30px;
    border: 0;
    cursor: pointer;
    border-radius: 5px;
    font-size: 16px;
    padding: 0 16px;
    line-height: 38px;
    font-weight: 600;
}

.com-selector button:hover, .com-answers button:hover{
    transition: 0.5s;
    background-color: blue;
}

.com-area{
    display: flex;
    align-items: center;
}

.com-selector select{
    background: #252122;
    color: #fff;
    margin: 3px;
    margin-left: 30px;
    border: 0;
    cursor: pointer;
    border-radius: 5px;
    font-size: 16px;
    padding: 8px 16px;
    line-height: 38px;
    font-weight: 600;
}

.com-answers{
    font-size: 17px;
    width: 600px;
    margin: 0 auto;
    margin-top: 30px;
    /* background-color: aqua; */
}

.com-answers-header{
    display: flex;
    justify-content: space-between;
    margin-bottom: 30px;
    align-items: center;
}

.com-ans-head-left{
    display: flex;
}

.com-ans-head-left h2 {
    background-color: white;
    margin-right: 20px;
    padding: 5px 10px;
}

.com-question {
    background-color: white;
    padding: 5px 10px;
}
</style>