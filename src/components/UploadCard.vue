<template>
    <div class="col card-container fit-height">
        <div class="card-header">Tải ảnh lên</div>
        <div>
            <q-file
                v-model="file"
                @update:model-value="handleFileChange"
                ref="fileInput"
                accept="image/*"
                class="hide-file-input"
            />

            <template v-if="!imagePreview">
                <div
                    @click="triggerFileInput"
                    ref="dropZoneRef"
                    class="column items-center justify-center q-mb-md upload-area"
                >
                    <img src="../assets/upload-file.png" width="320px" class="mb-8" />
                    <span class="mb-8 drag-drop-text">Kéo và thả tệp vào đây</span>
                    <span class="mb-8">hoặc</span>
                    <q-btn label="Tải lên" class="contained-btn" flat no-caps />
                </div>
            </template>

            <template v-else>
                <div class="column items-center justify-center q-mb-md preview-area">
                    <img :src="imagePreview" class="preview-image mb-8" />
                </div>
                <div class="row justify-end q-gutter-x-sm">
                    <q-btn label="Bỏ chọn" @click="handleDiscardImage" class="outlined-btn" flat no-caps />
                    <q-btn label="Nhận dạng" @click="handleOCRImage" class="contained-btn" flat no-caps />
                </div>
            </template>
        </div>
    </div>
</template>

<script>
import { useDropZone } from '@vueuse/core';

export default {
    name: 'UploadCard',
    emits: ['onOCRImage', 'onResetResult'],
    data() {
        return {
            file: null,
            imagePreview: '',
        };
    },
    methods: {
        triggerFileInput() {
            this.$refs.fileInput.$el.click();
        },

        handleFileChange(file) {
            if (!file) {
                this.$q.notify({
                    color: 'negative',
                    position: 'bottom-right',
                    message: 'Không tìm thấy ảnh cần nhận dạng',
                    icon: 'warning',
                    timeout: 1000,
                });
                return;
            }

            this.file = file;
            this.imagePreview = URL.createObjectURL(file);
        },

        handleDiscardImage() {
            this.file = null;
            URL.revokeObjectURL(this.imagePreview);
            this.imagePreview = null;

            this.$emit('onResetResult');
        },

        handleOCRImage() {
            this.$emit('onOCRImage', this.file);
        },

        onDrop(file) {
            this.file = file;
            this.imagePreview = URL.createObjectURL(file);
        },
    },
    mounted() {
        const dropZone = this.$refs.dropZoneRef;

        if (dropZone) {
            useDropZone(dropZone, {
                onDrop: (files) => {
                    this.onDrop(files[0]);
                },
                dataTypes: ['image/*'],
                multiple: false,
                preventDefaultForUnhandled: false,
            });
        }
    },
    beforeUnmount() {
        if (this.imagePreview) {
            URL.revokeObjectURL(this.imagePreview);
        }
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

.hide-file-input {
    display: none;
}

.upload-area {
    width: 100%;
    height: 400px;
    padding: 24px;
    border-radius: 8px;
    border: 1px dashed #cdcdcd;
    cursor: pointer;
}

.upload-area:hover {
    border: 1px dashed #0671e0;
    background: rgba(6, 113, 224, 0.1);
}

.mb-8 {
    margin-bottom: 8px;
}

.drag-drop-text {
    color: #0671e0;
    font-weight: 600;
}

.outlined-btn {
    padding: 8px 24px;
    border-radius: 4px;
    border: 1px solid #cdcdcd;
    background: white;
    font-weight: 600;
}

.contained-btn {
    padding: 8px 24px;
    border-radius: 4px;
    color: white;
    background: #0671e0;
    font-weight: 600;
}

.preview-area {
    width: 100%;
    height: 400px;
    border-radius: 8px;
}
.preview-image {
    max-width: 100%;
    max-height: 400px;
    object-fit: contain;
    border: 1px solid #eee;
    border-radius: 4px;
}
</style>
