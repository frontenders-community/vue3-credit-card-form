<script setup>
import { reactive, computed, watch } from 'vue';
import { vMaska } from "maska"

const emit = defineEmits(['inputChanged', 'cvvFocus', 'cvvBlur'])

const state = reactive({
    cardNumber: "",
    cardName: "",
    cardMonth: "",
    cardYear: "",
    minCardYear: new Date().getFullYear(),
    cardCvv: ""
});

const minCardMonth = computed(() => {
    if (state.cardYear === state.minCardYear)
        return new Date().getMonth() + 1;
    return 1;
})

watch(() => state.cardYear, async(newCardYear, oldCardYear) => {
    if(state.cardMonth < minCardMonth) {
        state.cardMonth = "";
        emit('inputChanged', { name: 'cardMonth', value: state.cardMonth })
    }
})

</script>

<template>
    <div class="card-form__inner">
        <div class="card-input">
            <label for="cardNumber" class="card-input__label">Card Number</label>
            <input type="text" id="cardNumber" class="card-input__input" data-ref="cardNumber" autocomplete="off" v-maska
                data-maska="#### #### #### ####" v-model="state.cardNumber"
                @keyup="$emit('inputChanged', { name: 'cardNumber', value: state.cardNumber })">
        </div>
        <div class="card-input">
            <label for="cardName" class="card-input__label">Card Holders</label>
            <input type="text" id="cardName" class="card-input__input" data-ref="cardName" autocomplete="off"
                v-model="state.cardName" @keyup="$emit('inputChanged', { name: 'cardName', value: state.cardName })">
        </div>
        <div class="card-form__row">
            <div class="card-form__col">
                <div class="card-form__group">
                    <label for="cardMonth" class="card-input__label">Expiration Date</label>
                    <select class="card-input__input select" id="cardMonth" data-ref="cardDate" v-model="state.cardMonth"
                        @change="$emit('inputChanged', { name: 'cardMonth', value: state.cardMonth })">
                        <option value="" disabled selected>Month</option>
                        <option :value="n < 10 ? '0' + n : n" v-for="n in 12" :disabled="n < minCardMonth" :key="n">
                            {{ n < 10 ? '0' + n : n }} </option>
                    </select>
                    <select class="card-input__input select" id="cardYear" data-ref="cardDate" v-model="state.cardYear"
                        @change="$emit('inputChanged', { name: 'cardYear', value: state.cardYear })">
                        <option value="" disabled selected>Year</option>
                        <option :value="index + state.minCardYear" v-for="(n, index) in 12" :key="n">
                            {{ index + state.minCardYear }}
                        </option>
                    </select>
                </div>
            </div>
            <div class="card-form__col cvv">
                <div class="card-input">
                    <label for="cardCvv" class="card-input__label">CVV</label>
                    <input type="text" class="card-input__input" id="cardCvv" v-maska data-maska="####"
                        v-model="state.cardCvv" maxlength="4" autocomplete="off" @focus="$emit('cvvFocus')" @blur="$emit('cvvBlur')"
                        @keyup="$emit('inputChanged', { name: 'cardCvv', value: state.cardCvv })">
                </div>
            </div>
        </div>

        <button class="card-form__button">
            Submit
        </button>
    </div>
</template>

<style scoped lang="scss">
@use "../assets/scss/partials/variables" as *;

.card-form {
    &__inner {
        background: #fff;
        box-shadow: 0 30px 60px 0 rgba(90, 116, 148, 0.4);
        border-radius: 10px;
        padding: 35px;
        padding-top: 180px;

        @media screen and (max-width: 480px) {
            padding: 25px;
            padding-top: 165px;
        }

        @media screen and (max-width: 360px) {
            padding: 15px;
            padding-top: 165px;
        }
    }

    &__row {
        display: flex;
        align-items: flex-start;

        @media screen and (max-width: 480px) {
            flex-wrap: wrap;
        }
    }

    &__col {
        flex: auto;
        margin-right: 35px;

        &:last-child {
            margin-right: 0;
        }

        @media screen and (max-width: 480px) {
            margin-right: 0;
            flex: unset;
            width: 100%;
            margin-bottom: 20px;
        }

        &.cvv {
            max-width: 150px;

            @media screen and (max-width: 480px) {
                max-width: initial;
            }
        }
    }

    &__group {
        display: flex;
        align-items: flex-start;
        flex-wrap: wrap;

        .card-input__input {
            flex: 1;
            margin-right: 15px;

            &:last-child {
                margin-right: 0;
            }
        }
    }

    &__button {
        width: 100%;
        padding: 15px 0;
        background: $color-primary;
        border: none;
        border-radius: 5px;
        font-size: 22px;
        font-weight: 500;
        box-shadow: 3px 10px 20px 0px rgba(35, 100, 210, 0.3);
        color: #fff;
        margin-top: 20px;
        cursor: pointer;
        transition: all .3s;

        &:hover {
            background-color: $color-primary-dark;
        }
    }
}

.card-input {
    margin-bottom: 20px;

    &__label {
        font-size: 14px;
        margin-bottom: 5px;
        font-weight: 500;
        color: #1a3b5d;
        width: 100%;
        display: block;
        user-select: none;
    }

    &__input {
        width: 100%;
        height: 50px;
        border-radius: 5px;
        box-shadow: none;
        border: 1px solid #ced6e0;
        transition: all 0.3s ease-in-out;
        font-size: 18px;
        padding: 5px 15px;
        background: none;
        color: #1a3b5d;
        font-family: "Source Sans Pro", sans-serif;

        &:hover,
        &:focus {
            border-color: #3d9cff;
        }

        &:focus {
            box-shadow: 0px 10px 20px -13px rgba(32, 56, 117, 0.35);
        }

        &.select {
            -webkit-appearance: none;
            background-image: url("../assets/img/chevron.png");
            background-size: 12px;
            background-position: 90% center;
            background-repeat: no-repeat;
            padding-right: 30px;
        }
    }
}</style>