<template>
    <q-page>
        <div class="text-center q-py-xl main-heading">Website nhận dạng các loại văn bản</div>

        <div class="row q-gutter-x-xl">
            <div class="col-2 card-container fit-height">
                <MenuItems v-model="selectedMenu" />
            </div>

            <UploadCard @onOCRImage="handleOCRImage" @onResetResult="handleResetResult" ref="uploadCardRef" />

            <ResultCard :ocrResult="ocrResult" />
        </div>
    </q-page>
</template>

<script>
import axios from 'axios';
import { Loading } from 'quasar';

// import { api } from '../boot/axios';
import { DOCUMENT_MAPPINGS } from 'src/constants/index';
import MenuItems from 'src/components/MenuItems.vue';
import UploadCard from 'src/components/UploadCard.vue';
import ResultCard from 'src/components/ResultCard.vue';

export default {
    name: 'IndexPage',
    components: { MenuItems, UploadCard, ResultCard },
    data() {
        return {
            selectedMenu: 'gthc',
            ocrResult: [],
        };
    },
    watch: {
        selectedMenu() {
            this.$refs.uploadCardRef.handleDiscardImage();
            this.ocrResult = [];
        },
    },
    methods: {
        handleResetResult() {
            this.ocrResult = [];
        },

        async handleOCRImage(file) {
            const formData = new FormData();
            formData.append('file', file);

            const request = {
                gthc: {
                    url: '/services/gthc',
                    method: 'post',
                    headers: { 'Content-Type': 'multipart/form-data' },
                    data: formData,
                },
                cmnd: {
                    url: '/services/cmnd',
                    method: 'post',
                    headers: { 'Content-Type': 'multipart/form-data' },
                    data: formData,
                },
                gplx: {
                    url: '/services/gplx',
                    method: 'post',
                    headers: { 'Content-Type': 'multipart/form-data' },
                    data: formData,
                },
                invoice: {
                    url: '/services/invoice',
                    method: 'post',
                    headers: { 'Content-Type': 'multipart/form-data' },
                    data: formData,
                },
            };

            const { url, method, headers, data } = request[this.selectedMenu];

            Loading.show({ message: 'Đang nhận dạng văn bản...' });

            try {
                const response = await axios.request({
                    url: `/api/${url}`,
                    method,
                    headers,
                    data,
                });

                if (response.status === 200) {
                    const { value } =
                        this.selectedMenu === 'cmnd' || this.selectedMenu === 'gplx'
                            ? (response?.data?.[0] ?? {})
                            : (response?.data ?? {});

                    const currentMapping = DOCUMENT_MAPPINGS[this.selectedMenu] || {};

                    this.ocrResult = Object.keys(value).map((key) => {
                        let val = value[key];

                        if (key === 'loai_vb' || key === 'lang') {
                            val = currentMapping[val];
                        } else if (key === 'noi_nhan_ben_ngoai' || key === 'noi_nhan_ben_trong') {
                            val = val.join(' ');
                        } else if (key === 'ngay_cap' || key === 'ngay_ban_hanh' || key === 'ngay_den') {
                            val = `${val.ngay}/${val.thang}/${val.nam}`;
                        }

                        return {
                            originalKey: key,
                            displayName: currentMapping[key] || key,
                            value: val,
                        };
                    });
                }
            } catch (error) {
                this.$q.notify({
                    color: 'negative',
                    position: 'bottom-right',
                    message: error?.response?.data?.message || 'Không thể nhận dạng được hình ảnh đã chọn',
                    icon: 'warning',
                    timeout: 1500,
                });
            } finally {
                Loading.hide();
            }
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

.fit-height {
    height: fit-content;
}
</style>
