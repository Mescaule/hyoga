<script lang="ts">
import { defineComponent } from 'vue';
import { VMoney } from 'v-money';
import { convertCurrencyToNumber } from '@/utils/Numbers';

export default defineComponent({
    name: 'MInputNumber',
    props: {
        modelValue: {
            type: Number
        },
        disabled: {
            type: Boolean,
            default: false
        },
        placeholder: {
            type: String,
            default: ''
        }
    },
    emits: ['update:modelValue'],
    data() {
        return {
            price: '',
            money: {
                decimal: ',',
                thousands: '.'
            }
        };
    },
    directives: { money: VMoney },
    watch: {
        modelValue: {
            immediate: true,
            handler(value) {
                this.price = value ? value.toLocaleString('pt-BR', { minimumFractionDigits: 2 }) : '';
            }
        }
    },
    methods: {
        updateValue() {
            this.$emit('update:modelValue', convertCurrencyToNumber(this.price));
        }
    },
    setup() {
        return {};
    }
});
</script>

<template>
    <span class="p-inputnumber p-component p-inputwrapper p-inputwrapper-filled" data-pc-name="inputnumber" data-pc-section="root">
        <input inputmode="decimal" class="p-inputtext p-component p-inputnumber-input" :disabled="disabled" :placeholder="placeholder" v-money="money" v-model.lazy="price" @blur="updateValue" />
    </span>
</template>

<style scoped></style>
