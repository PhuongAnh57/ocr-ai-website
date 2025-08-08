<template>
    <div class="col card-container result-card">
        <div class="card-header">Kết quả nhận dạng</div>

        <div>
            <span v-if="!ocrResult?.length" class="flex flex-center text-italic">Không có dữ liệu</span>

            <template v-else>
                <div v-for="(item, index) in ocrResult" :key="index" class="row q-mb-lg">
                    <span style="min-width: 240px">{{ item.displayName }}:</span>
                    <span class="col text-bold">{{ item.value }}</span>
                </div>
            </template>
        </div>
    </div>
</template>

<script>
export default {
    name: 'ResultCard',
    props: {
        ocrResult: {
            type: Array,
            default: () => [],
        },
    },
};
</script>

<style scoped>
.card-container {
    padding: 24px;
    border-radius: 8px;
    box-shadow: 0 0 24px rgba(149, 157, 165, 0.2);
    background: white;
}
.card-header {
    margin-bottom: 24px;
    font-size: 20px;
    font-weight: bold;
}

.fit-height {
    height: fit-content;
}
</style>
