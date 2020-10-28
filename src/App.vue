<template>
<div id="app">
    <Confirmation v-if="submitted && confirmed" />
    <form v-show="!confirmed" class="form" @submit.prevent="submitHandler">
        <fieldset class="grid_1">
            <legend>Основные данные</legend>
            <input id="name" name="name" type="text" placeholder="Савелий" v-model.trim="name" :class="[

(!$v.name.required && $v.name.$dirty) ||

($v.name.$dirty && !$v.name.minLength)

? 'invalid'

: 'valid',

]" />
            <label for="name">Имя*</label>
            <div class="input--error name" v-if="!$v.name.required && $v.name.$dirty">
                Обязательное поле
            </div>
            <input id="surname" name="surname" type="text" placeholder="Грядкин" v-model.trim="surname" :class="[

(!$v.surname.required && $v.surname.$dirty) ||

($v.surname.$dirty && !$v.surname.minLength)

? 'invalid'

: 'valid',

]" />
            <label for="surname">Фамилия*</label>
            <div class="input--error surname" v-if="!$v.surname.required && $v.surname.$dirty">
                Обязательное поле
            </div>
            <input id="patronymic" name="patronymic" type="text" placeholder="Павлович" v-model.trim="patronymic" />
            <label for="patronymic">Отчество</label>
            <div class="input--error phone_number" v-if="$v.phone_number.$dirty && !$v.phone_number.required">
                Обязательное поле
            </div>
            <div class="input--error phone_number" v-if="$v.phone_number.$dirty && !$v.phone_number.isNumber">
                Введите номер в формате +79999999999
            </div>
            <input id="phone_number" name="phone_number" type="tel" placeholder="+7(965)-667-88-77" @input="formatNumber" v-model.trim="phone_number" :class="[

($v.phone_number.$dirty && !$v.phone_number.isNumber) ||

($v.phone_number.$dirty && !$v.phone_number.required)

? 'invalid'

: 'valid',

]" />
            <label for="phone_number">Телефон*</label>
            <div class="radioBox">
                <CustomRadio v-model="sex" />
            </div>
            <select class="clientGroup" v-model="clientGroup" :class="[

!$v.clientGroup.required && $v.clientGroup.$dirty

? 'invalid'

: 'valid',

]">
                <option>VIP</option>
                <option>Проблемные</option>
                <option>ОМС</option>
            </select>
            <span class="clientGroup_tip">Группа клиентов*</span>
            <div class="input--error clientGroup" v-if="!$v.clientGroup.required && $v.clientGroup.$dirty">
                Обязательное поле
            </div>
            <select class="clientDoctor" v-model="clientDoctor">
                <option>Захаров</option>
                <option>Чернышева</option>
                <option>Иванов</option>
            </select>
            <span class="clientDoctor_tip">Лечащий врач</span>
            <div class="checkBox">
                <ToggleButton v-model="unsubscribe" />
                <span>Не отправлять СМС</span>
            </div>
        </fieldset>
        <fieldset class="grid_2">
            <legend for="address">Адрес</legend>
            <div id="address_row">
                <div class="address_row--item">
                    <input id="street" name="street" type="text" placeholder="Циолковского" v-model="address.street" />
                    <label for="street">Улица </label>
                </div>
                <div class="address_row--item">
                    <input id="building" name="building" type="text" placeholder="100" v-model="address.building" />
                    <label for="building">Дом</label>
                </div>
                <div class="address_row--item">
                    <input id="flat" name="flat" type="text" placeholder="350" v-model="address.flat" />
                    <label for="flat">Квартира </label>
                </div>
            </div>
            <input id="city" name="city" type="text" placeholder="Новосибирск" v-model="address.city" :class="[

$v.address.city.$dirty && !$v.address.city.required

? 'invalid'

: 'valid',

]" />
            <label for="city">Город*</label>
            <div class="input--error city" v-if="$v.address.city.$dirty && !$v.address.city.required">
                Обязательное поле
            </div>
            <input id="region" name="region" type="text" placeholder="Новосибирская область" v-model="address.region" />
            <label for="region">Область</label>
            <input id="index" name="index" type="text" placeholder="123456" v-model="address.index" />
            <label for="index">Индекс</label>
            <div class="input--error index" v-if="

$v.address.index.$dirty &&

(!$v.address.index.minLength ||

!$v.address.index.maxLength)

">
                Индекс должен состоять из 6 символов
            </div>
            <input id="country" name="country" type="text" placeholder="Россия" v-model="address.country" />
            <label for="country">Страна</label>
        </fieldset>
        <fieldset class="grid_3">
            <legend>Документ</legend>
            <input id="doc_type" name="doc_type" type="text" placeholder="паспорт" v-model="documents.doc_type" :class="[

$v.documents.doc_type.$dirty &&

!$v.documents.doc_type.required

? 'invalid'

: 'valid',

]" />
            <label for="doc_type">Тип документа*</label>
            <div class="input--error doc_type" v-if="

$v.documents.doc_type.$dirty &&

!$v.documents.doc_type.required

">
                Обязательное поле
            </div>
            <input id="serial_num" name="serial_num" type="text" placeholder="4200" v-model="documents.serial_num" />
            <label for="serial_num">Серия документа</label>
            <input id="doc_num" name="doc_num" type="text" placeholder="899699" v-model="documents.doc_num" />
            <label for="doc_num">Номер</label>
            <input id="issued_by" name="issued_by" type="text" placeholder="отделением №1  УФМС России  г.Новосибирска" v-model="documents.issued_by" />
            <label for="issued_by">Кем выдан</label>
            <input id="date_of_issue" name="date_of_issue" type="text" @input="formatDate" placeholder="01.01.1991" v-model="documents.date_of_issue" :class="[

$v.documents.date_of_issue.$dirty &&

!$v.documents.date_of_issue.required

? 'invalid'

: 'valid',

]" />
            <label for="date_of_issue">Дата выдачи*</label>
            <div class="input--error date_of_issue" v-if="

$v.documents.date_of_issue.$dirty &&

!$v.documents.date_of_issue.required

">
                Обязательное поле
            </div>
            <div class="input--error date_of_issue" v-if="

$v.documents.date_of_issue.$dirty &&

!$v.documents.date_of_issue.isFormatDate

">
                Проверьте введенные данные.
            </div>
        </fieldset>
        <button type="submit">Отправить</button>
    </form>
</div>
</template>

<script>
import {
    required,
    minLength,
    maxLength,
    helpers,
    numeric,
} from 'vuelidate/lib/validators';
import ToggleButton from '@/components/ToggleButton.vue';
import CustomRadio from '@/components/CustomRadioBtn.vue';
import Confirmation from '@/components/Confirmation.vue';

const isNumber = helpers.regex(
    'isNumber',
    /^\+(7)\(([0-9]{3})\)\-([0-9]{3})\-([0-9]{2})\-([0-9]{2})$/
);
const isFormatDate = helpers.regex(
    'isFormatDate',
    /^([0-1][0-9])\.([0-1][0-9])\.([0-9]{4})$/
);
export default {
    name: 'App',
    components: {
        ToggleButton,
        CustomRadio,
        Confirmation,
    },
    data() {
        return {
            name: '',
            surname: '',
            patronymic: '',
            phone_number: '+7',
            sex: '',
            unsubscribe: false,
            clientGroup: '',
            clientDoctor: '',
            address: {
                street: '',
                building: '',
                flat: '',
                city: '',
                region: '',
                index: '',
                country: '',
            },
            documents: {
                doc_type: '',
                serial_num: '',
                doc_num: '',
                issued_by: '',
                date_of_issue: '',
            },
            confirmed: false,
            submitted: false,
        };
    },
    validations: {
        name: {
            required,
            minLength: minLength(2),
        },
        surname: {
            required,
            minLength: minLength(2),
        },
        patronymic: {
            minLength: minLength(5),
        },
        sex: {},
        unsubscribe: {},
        phone_number: {
            required,
            isNumber,
        },
        clientGroup: {
            required,
        },
        clientDoctor: {},
        address: {
            street: {},
            city: {
                required,
            },
            region: {},
            index: {
                numeric,
                minLength: minLength(6),
                maxLength: maxLength(6),
            },
            country: {},
        },
        documents: {
            doc_type: {
                required,
            },
            serial_num: {},
            doc_num: {},
            issued_by: {},
            date_of_issue: {
                required,
                isFormatDate,
            },
        },
    },
    methods: {
        status(validation) {
            return {
                error: validation.$error,
                dirty: validation.$dirty,
            };
        },
        acceptNumber(number) {
            const regex = /^\+(7)\(([0-9]{3})\)\-([0-9]{3})\-([0-9]{2})\-([0-9]{2})$/;
            return regex.test(number);
        },
        formatNumber() {
            if (this.phone_number.match(/^(\+7)([0-9]{3})$/)) {
                this.phone_number = this.phone_number.replace(
                    /^(\+7)([0-9]{3})$/,
                    '$1($2)-'
                );
            } else if (
                this.phone_number.match(/^(\+7)\(([0-9]{3})\)-([0-9]{3})$/)
            ) {
                this.phone_number = this.phone_number.replace(
                    /^(\+7)\(([0-9]{3})\)-([0-9]{3})$/,
                    '$1($2)-$3-'
                );
            } else if (
                this.phone_number.match(
                    /^(\+7)\(([0-9]{3})\)-([0-9]{3})-([0-9]{2})$/
                )
            ) {
                this.phone_number = this.phone_number.replace(
                    /^(\+7)\(([0-9]{3})\)-([0-9]{3})-([0-9]{2})$/,
                    '$1($2)-$3-$4-'
                );
            } else if (
                this.phone_number.match(
                    /^(\+7)([0-9]{3})([0-9]{3})([0-9]{2})([0-9]{2})$/
                )
            ) {
                this.phone_number = this.phone_number.replace(
                    /^(\+7)([0-9]{3})([0-9]{3})([0-9]{2})([0-9]{2})$/,
                    '$1($2)-$3-$4-$5'
                );
            }
        },
        formatDate() {
            console.log(this.documents.date_of_issue);
            this.documents.date_of_issue.match(/^([0-9]{2})$/) ?
                (this.documents.date_of_issue = this.documents.date_of_issue.replace(
                    /([0-9]{2})$/,
                    '$1.'
                )) :
                this.documents.date_of_issue.match(/^([0-9]{2})\.([0-9]{2})$/) ?
                (this.documents.date_of_issue = this.documents.date_of_issue.replace(
                    /^([0-9]{2})\.([0-9]{2})$/,
                    '$1.$2.'
                )) :
                false;
        },
        submitHandler() {
            this.submitted = true;

            if (this.$v.$invalid) {
                this.$v.$touch();
                return;
            } else {
                this.confirmed = true;
            }
        },
    },

    computed: {},
};
</script>

<style lang="scss">
@import 'normalize-scss';
@include normalize();
@import './assets/scss/fonts';

body {
    background: linear-gradient(#2b5876, #4e4376);
    overflow: hidden;
}

* {
    box-sizing: border-box;
}

#app {
    font-family: 'Montserrat', Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    justify-items: center;
}

.form {
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    align-content: center;
    width: 700px;
    padding: 20px 40px;

    border-radius: 20px;
    background: #fff;
    box-shadow: 1px 1px 20px #2c3e50;

    fieldset {
        border: 1px solid lighten($color: #2b5876, $amount: 10);
        border-radius: 14px;
        width: 95%;
        margin: 20px auto;

        legend {
            align-self: center;
            justify-self: center;
            padding: 10px;
            color: #4e4376;
        }

        input,
        select {
            -webkit-appearance: none;
            border: 1px solid rgba(31, 32, 65, 0.25);
            background-color: #fff;
            border-radius: 6px;
            padding: 5px 10px;
            color: rgba(31, 32, 65, 0.75);
            outline: none;
            cursor: pointer;

            &:hover {
                border: 1px solid #4e4376;
            }

            &.invalid {
                border: 1px solid lightcoral;
            }

            &::placeholder {
                color: rgba(31, 32, 65, 0.5);
            }
        }

        label,
        span {
            color: rgba(31, 32, 65, 0.75);
            font-size: 10px;
            justify-self: flex-start;
            user-select: none;
        }
    }

    .grid_1 {
        display: grid;
        height: 240px;
        grid-template-columns: 30px 160px auto 160px auto 160px 30px;
        grid-template-rows: repeat(8, 30px);

        #name {
            grid-row: 2/3;
            grid-column: 2/3;
        }

        label[for='name'] {
            grid-row: 3/4;
            grid-column: 2/3;
        }

        .input--error.name {
            grid-row: 1/2;
            grid-column: 2/3;
            align-self: flex-end;
            padding-bottom: 3px;
        }

        #surname {
            grid-row: 2/3;
            grid-column: 4/5;
        }

        label[for='surname'] {
            grid-row: 3/4;
            grid-column: 4/5;
        }

        .input--error.surname {
            grid-row: 1/2;
            grid-column: 4/5;
            align-self: flex-end;
            padding-bottom: 3px;
        }

        #patronymic {
            grid-row: 2/3;
            grid-column: 6/7;
        }

        label[for='patronymic'] {
            grid-row: 3/4;
            grid-column: 6/7;
        }

        #phone_number {
            grid-row: 4/5;
            grid-column: 2/3;
        }

        label[for='phone_number'] {
            grid-row: 5/6;
            grid-column: 2/3;
        }

        .input--error.phone_number {
            grid-row: 3/4;
            grid-column: 2/3;
            align-self: flex-end;
            padding-bottom: 3px;
        }

        .clientDoctor {
            grid-row: 4/5;
            grid-column: 4/5;
        }

        .clientDoctor_tip {
            grid-row: 5/6;
            grid-column: 4/5;
        }

        .clientGroup {
            grid-row: 4/5;
            grid-column: 6/7;
        }

        .input--error.clientGroup {
            grid-row: 3/4;
            grid-column: 6/7;
            align-self: flex-end;
            padding-bottom: 3px;
        }

        .clientGroup_tip {
            grid-row: 5/6;
            grid-column: 6/7;
        }

        .radioBox {
            grid-row: 6/7;
            grid-column: 2/3;
            display: flex;
        }

        .checkBox {
            grid-row: 6/7;
            grid-column: 4/5;
            display: flex;
            justify-content: flex-start;
            align-items: center;

            span {
                padding-left: 10px;
            }
        }
    }

    .grid_2 {
        display: grid;
        grid-template-rows: repeat(7, 30px);
        grid-template-columns: 30px 225px auto 225px 30px;

        #address_row {
            grid-column: 2/5;
            grid-row: 2/3;
            display: flex;
            justify-content: space-between;

            .address_row--item {
                display: flex;
                flex-direction: column;
                width: 160px;

                label {
                    align-self: flex-start;
                }
            }
        }

        #city {
            grid-column: 2/3;
            grid-row: 4/5;
        }

        label[for='city'] {
            grid-column: 2/3;
            grid-row: 5/6;
        }

        .input--error.city {
            grid-column: 2/3;
            grid-row: 3/4;
            align-self: flex-end;
            padding-bottom: 3px;
        }

        #region {
            grid-column: 4/5;
            grid-row: 4/5;
        }

        label[for='region'] {
            grid-column: 4/5;
            grid-row: 5/6;
        }

        #index {
            grid-column: 2/3;
            grid-row: 6/7;
        }

        label[for='index'] {
            grid-column: 2/3;
            grid-row: 7/8;
        }

        .input--error.index {
            align-self: flex-end;
            padding-bottom: 3px;
            grid-column: 2/3;
            grid-row: 5/6;
        }

        #country {
            grid-column: 4/5;
            grid-row: 6/7;
        }

        label[for='country'] {
            grid-column: 4/5;
            grid-row: 7/8;
        }
    }

    .grid_3 {
        display: grid;
        grid-template-rows: repeat(7, 30px);
        grid-template-columns: 30px 225px auto 225px 30px;

        #doc_type {
            grid-row: 2/3;
            grid-column: 2/3;
        }

        label[for='doc_type'] {
            grid-row: 3/4;
            grid-column: 2/3;
        }

        .input--error.doc_type {
            grid-row: 1/2;
            grid-column: 2/3;
            align-self: flex-end;
            padding-bottom: 3px;
        }

        #serial_num {
            grid-row: 2/3;
            grid-column: 4/5;
        }

        label[for='serial_num'] {
            grid-row: 3/4;
            grid-column: 4/5;
        }

        #doc_num {
            grid-row: 4/5;
            grid-column: 4/5;
        }

        label[for='doc_num'] {
            grid-row: 5/6;
            grid-column: 4/5;
        }

        #issued_by {
            grid-row: 6/7;
            grid-column: 2/5;
        }

        label[for='issued_by'] {
            grid-row: 7/8;
            grid-column: 2/5;
        }

        #date_of_issue {
            grid-row: 4/5;
            grid-column: 2/3;
        }

        label[for='date_of_issue'] {
            grid-row: 5/6;
            grid-column: 2/3;
        }

        .input--error.date_of_issue {
            grid-row: 3/4;
            grid-column: 2/3;
            align-self: flex-end;
            padding-bottom: 3px;
        }
    }

    button[type='submit'] {
        width: 320px;
        height: 44px;
        background: #4e4376;
        border-radius: 22px;
        border: none;
        color: white;
        text-transform: uppercase;
        outline: none;

        &:hover {
            background: lighten($color: #4e4376, $amount: 10);
        }
    }

    .input--error {
        color: #c94b4b;
        font-size: 7px;
    }
}

@import './assets/scss/mobile.scss';
</style>
