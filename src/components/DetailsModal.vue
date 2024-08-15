<script setup>
import { defineProps, defineEmits } from 'vue';

const props = defineProps({
    show: Boolean,
    details: Object,
});

const emit = defineEmits(['close']);

const closeModal = () => {
    emit('close');
};
</script>

<template>
    <div class="modal-overlay" v-if="show">
        <div class="modal-content">
            <h3 class="modal-heading">Details</h3>
            <p><strong>Description:</strong> {{ details.Description }}</p>
            <p><strong>Growth Rate:</strong> {{ details.GrowthRate }}</p>
            <p><strong>Target Market:</strong> {{ details.TargetMarket }}</p>
            <button class="modal-btn" @click="closeModal">Close</button>
        </div>
    </div>
</template>

<style scoped>
p{
    margin-bottom: 10px;
}

.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background-color: white;
  padding: 20px;
  border-radius: 5px;
}

.modal-btn{
    background: #108a00;
    color: #fff;
    margin: 3px;
    margin-top:30px;
    border: 0;
    cursor: pointer;
    border-radius: 5px;
    font-size: 16px;
    padding: 0 16px;
    line-height: 38px;
    font-weight: 600;
}

.modal-btn:hover{
    background: #0b6100;
    transition: 0.3s;
}

.modal-heading{
    margin-bottom: 30px;
}
</style>