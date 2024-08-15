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
            <h3>Details</h3>
            <p><strong>Description:</strong> {{ details.Description }}</p>
            <p><strong>Growth Rate:</strong> {{ details.GrowthRate }}</p>
            <p><strong>Target Market:</strong> {{ details.TargetMarket }}</p>
            <button @click="closeModal">Close</button>
        </div>
    </div>
</template>

<style>
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
</style>