<template>
    <q-list class="q-gutter-y-sm">
        <q-item
            v-for="item in menuItems"
            :key="item.value"
            clickable
            class="menu-item"
            :active="selectedMenu === item.value"
            active-class="item-active"
            @click="setSelectedMenu(item.value)"
        >
            <q-item-section>{{ item.label }}</q-item-section>
        </q-item>
    </q-list>
</template>

<script>
export default {
    name: 'MenuItems',
    emits: ['update:modelValue'],
    props: {
        modelValue: {
            type: String,
            default: 'gthc',
        },
    },
    computed: {
        selectedMenu: {
            get() {
                return this.modelValue;
            },
            set(value) {
                this.$emit('update:modelValue', value);
            },
        },
    },
    data() {
        return {
            menuItems: [
                { value: 'gthc', label: 'Văn bản hành chính' },
                { value: 'cmnd', label: 'Chứng minh nhân dân' },
                { value: 'gplx', label: 'Giấy phép lái xe' },
                { value: 'invoice', label: 'Hóa đơn' },
            ],
        };
    },
    methods: {
        setSelectedMenu(value) {
            this.selectedMenu = value;
        },
    },
};
</script>

<style scoped>
.menu-item {
    height: 40px;
    border-radius: 8px;
    overflow: hidden;
}

.menu-item:hover {
    background-color: rgba(194, 194, 194, 0.1);
}

.item-active {
    color: #0671e0;
    font-weight: 600;
    background-color: rgba(6, 113, 224, 0.1);
}
</style>
