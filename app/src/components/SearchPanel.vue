<template>
    <div class="wrapper">
        <div class="search-panel">
            <v-sheet style="max-width: 700px;">
                <v-form fast-fail @submit.prevent class="search-form">
                    <div class="search-form-fields">
                        <v-text-field
                            v-model="location"
                            label="Локация"
                            width="200"
                            color="primary"
                            :rules="locationRules"
                            required
                        ></v-text-field>

                        <v-text-field
                            type="date"
                            v-model="filterForm.date_from"
                            label="От"
                            :rules="dateRules"
                            required
                        ></v-text-field>

                        <v-text-field
                            type="date"
                            v-model="filterForm.date_to"
                            label="До"
                            :rules="dateRules"
                            required
                        ></v-text-field>
                    </div>

                    <div class="divider"></div>

                    <v-btn block color="green" type="submit" @click="submitFilters">
                        Найти
                    </v-btn>

                </v-form>
            </v-sheet>
        </div>
        <div class="close-btn">
            <v-btn variant="text" @click="closePanel" icon="mdi-close" color="red"</v-btn>
        </div>
    </div>
    
</template>

<script setup lang="ts">
    import { ref } from 'vue';
    
    import { useBaseStore } from '../store/modules/base';
    import { dateRules, locationRules } from '../utils/rules';

    const baseStore = useBaseStore();

    const emit = defineEmits(['closedPanel'])

    const location = ref(null);

    const filterForm = ref({
        date_from: null,
        date_to: null
    });

    const submitFilters = async () => {
        if (location.value && filterForm.value.date_from && filterForm.value.date_to) {
            await baseStore.fetchHotelsByFilters(location.value, filterForm.value);
        }
    }

    const closePanel = () => {
        emit('closedPanel');
    }
</script>

<style scoped>
    @keyframes show{
        0%{
            opacity:0;
        }
        100% {
            opacity:1;
            transform: translateY(0px);
        }
    }

    .search-panel {
        position: relative;
    }

    .search-form-fields {
        display: flex;
        gap: 20px;
    }

    .wrapper {
        position: relative;
        transform: translateY(-50px);
        opacity: 0;
        transition: 1s;
        animation: show 0.5s 1;
        animation-fill-mode: forwards;
    }

    .close-btn {
        display: flex;
    }

    @media (max-width: 700px) {
        .search-form-fields {
            display: flex;
            flex-direction: column;
            width: 200px;
            gap: 10px;
        }
    }
</style>