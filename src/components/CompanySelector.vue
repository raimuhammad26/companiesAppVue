<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';

const companies = ref([]);

onMounted(async () => {
    try {
        const response = await axios.get('http://127.0.0.1:5000/company_list');
        companies.value = response.data.companies;
    } catch (error) {
        console.error('Error fetching company data:', error);
    }
});
</script>

<template>

<!-- Company Selector -->
    <div class="com-selector">
        <label for="companies">Choose a company:</label>

        <select name="companies" id="companies">
            <option v-for="company in companies" :key="company.name" :value="company.name">
                {{ company.name }}
            </option>
        </select>

        <div>
            <button>Area 1</button>
            <button>Topic 1</button>
            <button>Topic 2</button>
        </div>

        <div>
            <button>Area 2</button>
            <button>Topic 3</button>
        </div>
    </div>

    <!-- Company Answers -->
     <div class="com-answers">
        <div class="com-answers-header">
            <div>
                <div>Company Name</div>
                <div>Topic 1</div>
            </div>

            <div>
                <button>Back</button>
            </div>
        </div>

        <div class="com-question">
            <p>What is the TAM of the company?</p>
        </div>

        <div class="com-answers-table">

        </div>
     </div>


</template>