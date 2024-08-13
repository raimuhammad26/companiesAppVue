<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';

const companies = ref([]);

const selectedCompany = ref('');
const selectedTopic = ref('');
const showAnswers = ref(false);


onMounted(async () => {
    try {
        const response = await axios.get('http://127.0.0.1:5000/company_list');
        companies.value = response.data.companies;
        selectedCompany.value = companies.value[0].name;
    } catch (error) {
        console.error('Error fetching company data:', error);
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

        <div>
            <button>Area 1</button>
            <button @click="selectTopic(selectedCompany, 'Topic 1')">Topic 1</button>
            <button @click="selectTopic(selectedCompany, 'Topic 2')">Topic 2</button>
        </div>

        <div>
            <button>Area 2</button>
            <button  @click="selectTopic(selectedCompany, 'Topic 3')">Topic 3</button>
        </div>
    </div>

    <!-- Company Answers -->
    <div v-if="showAnswers" class="com-answers">
        <div class="com-answers-header">
            <div>
                <div>{{ selectedCompany }}</div>
                <div>{{ selectedTopic }}</div>
            </div>

            <div>
                <button @click="goBack">Back</button>
            </div>
        </div>

        <div class="com-question">
            <p>What is the TAM of the company?</p>
        </div>

        <div class="com-answers-table">

        </div>
    </div>


</template>