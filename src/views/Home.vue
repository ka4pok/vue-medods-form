<template>
    <div class="home">
        <h3 class="home__title">1. Сверстать форму создания Клиента</h3>
        <div class="wrapper">

            <div class="row">
                <form class="col s12" @submit.prevent="submitHandler">
                    <div class="row">

                        <div class="input-field "> <!-- FIRST NAME-->
                            <input placeholder="Placeholder" id="first_name" type="text"
                                   :class="{invalid: (this.$v.first_name.$dirty && !this.$v.first_name.required)}"
                                   v-model="first_name" autocomplete="off">
                            <label for="first_name" class="active">Фамилия*</label>
                            <small v-if="this.$v.first_name.$dirty && !this.$v.first_name.required"
                                   class="helper-text invalid"
                            >Поле Фамилия не должно быть пустым!</small>
                        </div>

                        <div class="input-field "><!--LAST NAME-->
                            <input id="last_name" type="text" autocomplete="off" v-model="last_name"
                                   :class="{invalid: (this.$v.last_name.$dirty && !this.$v.last_name.required)}">
                            <label for="last_name">Имя*</label>
                            <small v-if="this.$v.last_name.$dirty && !this.$v.last_name.required"
                                   class="helper-text invalid"
                            >Поле Имя не должно быть пустым!</small>
                        </div>

                        <div class="input-field "> <!--Patronymic-->
                            <input id="patronymic" type="text" v-model="patronymic"
                                   @change="$v.$touch()">
                            <label for="patronymic">Отчество</label>
                        </div>

                        <div class="input-field "> <!--DateOfBirth-->
                            <input id="dateOfBirth" type="text" ref="dateOfBirth" class="datepicker"
                                   @change="setDateOfBirth"
                                   :class="{invalid: (this.$v.dateOfBirth.$dirty && !this.$v.dateOfBirth.required )}">
                            <label for="dateOfBirth">Дата рождения*</label>
                            <small v-if="(this.$v.dateOfBirth.$dirty && !this.$v.dateOfBirth.required)"
                                   class="helper-text invalid"
                            >Поле Дата не должно быть пустым!</small>
                        </div>

                        <div class="input-field ">
                            <i class="material-icons prefix">phone</i>
                            <input id="phone" type="tel" data-length="11" ref="phone"
                                   autocomplete="off" v-model="phone"
                                   :class="{invalid: (this.$v.phone.$dirty && !this.$v.phone.required) || (this.$v.phone.$dirty && !this.$v.phone.numeric) || (this.$v.phone.$dirty && phone[0]!=='7') || (this.$v.phone.$dirty && phone.length!==11)}">
                            <label for="phone">Номер телефона*</label>
                            <small v-if="this.$v.phone.$dirty && !this.$v.phone.required"
                                   class="helper-text invalid"
                            >Поле Номер Телефона не должно быть пустым!</small>
                            <small v-else-if="this.$v.phone.$dirty && phone[0]!=='7'"
                                   class="helper-text invalid"
                            >Телефон должен начинаться с цифры 7!</small>
                            <small v-else-if="this.$v.phone.$dirty && !this.$v.phone.numeric"
                                   class="helper-text invalid"
                            >Телефон должен содержать только цифры!</small>
                            <small v-else-if="this.$v.phone.$dirty && phone.length!==11"
                                   class="helper-text invalid"
                            >Телефон должен содержать 11 цифр!</small>

                        </div>

                        <p>
                            <label>
                                <input name="group1" type="radio" value="male" v-model="sex"/>
                                <span>Мужчина</span>
                            </label>
                        </p>
                        <p>
                            <label>
                                <input name="group1" type="radio" value="female" v-model="sex"/>
                                <span>Женщина</span>
                            </label>
                        </p>

                        <div class="input-field ">
                            <select multiple ref="selectCustomer" v-model="customerGroup"
                                    @change="setSelectCustomer($event)"
                                    :class="{invalid: this.$v.$dirty && !this.$v.customerGroup.required}">
                                <option value="" disabled>Выберите группу клиентов</option>
                                <option value="1">VIP</option>
                                <option value="2">Проблемные</option>
                                <option value="3">ОМС</option>
                            </select>
                            <label>Группа клиентов*</label>
                            <small v-if="(this.$v.$dirty && !this.$v.customerGroup.required)"
                                   class="helper-text invalid "
                            >Вы должны выбрать хотя бы одну группу клиентов</small>
                        </div>

                        <div class="input-field">
                            <select ref="selectDoctor" v-model="doctor">
                                <option value="" disabled selected>Выберите лечащего врача</option>
                                <option value="1">Иванов</option>
                                <option value="2">Захаров</option>
                                <option value="3">Чернышева</option>
                            </select>
                            <label>Лечащий врач</label>
                        </div>

                        <p>
                            <label>
                                <input type="checkbox" v-model="sendSms"/>
                                <span>Не отправлять СМС</span>
                            </label>
                        </p>

                        <h4>Адрес:</h4>

                        <div class="input-field ">
                            <input id="index" type="text" v-model="index"
                                   :class="{invalid: (this.$v.index.$dirty && !this.$v.index.numeric)}">
                            <label for="index">Индекс</label>
                            <small v-if="this.$v.index.$dirty && !this.$v.index.numeric"
                                   class="helper-text invalid"
                            >Поле Индекс должно состоять из цифр</small>
                        </div>

                        <div class="input-field ">
                            <input id="country" type="text" v-model="country">
                            <label for="country">Страна</label>
                        </div>

                        <div class="input-field ">
                            <input id="region" type="text" v-model="region">
                            <label for="region">Область</label>
                        </div>

                        <div class="input-field ">
                            <input id="city" type="text" v-model="city"
                                   :class="{invalid:(this.$v.city.$dirty && !this.$v.city.required)}">
                            <label for="city">Город*</label>
                            <small v-if="this.$v.city.$dirty && !this.$v.city.required"
                                   class="helper-text invalid"
                            >Поле Город не должно быть пустым!</small>
                        </div>


                        <div class="input-field ">
                            <input id="street" type="text" v-model="street">
                            <label for="street">Улица</label>
                        </div>

                        <div class="input-field ">
                            <input id="house" type="text" v-model="house">
                            <label for="house">Дом</label>
                        </div>

                        <h4>Паспорт:</h4>

                        <div class="input-field">
                            <select ref="selectDocument" v-model="documentType"
                                    @change="setSelectDocument($event)"
                                    :class="{invalid: this.$v.documentType.$dirty && !this.$v.documentType.required}">
                                <option value="" disabled> Выберите тип документа</option>
                                <option value="1">Паспорт</option>
                                <option value="2">Свидетельство о рождении</option>
                                <option value="3">Вод. удостоверение</option>
                            </select>
                            <label>Тип документа*</label>
                            <small v-if="this.$v.$dirty && !this.$v.documentType.required"
                                   class="helper-text invalid"
                            >Вы должны выбрать тип документа!</small>
                        </div>

                        <div class="input-field ">
                            <input id="series" type="text" v-model="series">
                            <label for="series">Серия</label>
                        </div>

                        <div class="input-field ">
                            <input id="number" type="text" v-model=number>
                            <label for="number">Номер</label>
                        </div>

                        <div class="input-field ">
                            <input id="issuedBy" type="text" v-model="issuedBy">
                            <label for="issuedBy">Кем выдан</label>
                        </div>

                        <div class="input-field ">
                            <input id="dateOfIssue" type="text" class="datepicker"
                                   ref="dateOfIssue" @change="setDateOfIssue"
                                   :class="{invalid: this.$v.dateOfIssue.$dirty && !this.$v.dateOfIssue.required}">

                            <label for="dateOfIssue">Дата выдачи*</label>
                            <small v-if="this.$v.dateOfIssue.$dirty && !this.$v.dateOfIssue.required"
                                   class="helper-text invalid"
                            > Дата выдачи должна быть заполнена!!! </small>
                        </div>
                        <button type="submit" class="btn">Отправить</button>
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>

<script>

    import {required, numeric} from 'vuelidate/lib/validators'

    export default {
        name: 'Home',
        data() {
            return {
                first_name: '',
                last_name: '',
                patronymic: '',
                dateOfBirth: "",
                phone: '',
                sex: '',
                customerGroup: [],
                doctor: '',
                sendSms: false,
                index: '',
                country: '',
                region: '',
                city: '',
                street: '',
                house: '',
                documentType: '',
                series: '',
                number: '',
                issuedBy: '',
                dateOfIssue: ''
            }
        },
        validations: {
            first_name: {
                required,
            },
            last_name: {
                required,
            },
            dateOfBirth: {
                required
            },
            phone: {
                required,
                numeric
            },
            index: {
                numeric
            },
            customerGroup: {
                required
            },
            city: {
                required
            },
            documentType: {
                required
            },
            dateOfIssue: {
                required
            }

        },
        methods: {
            setSelectCustomer(event) {

                let selectCustomerParent = this.$refs.selectCustomer.parentElement
                if (this.$v.$dirty && !this.$v.customerGroup.required) {
                    selectCustomerParent.classList.add('invalid')
                } else if (this.$v.$dirty && this.$v.customerGroup.required) {
                    selectCustomerParent.classList.remove('invalid')
                }


            },
            setSelectDocument() {

                let selectDocumentParent = this.$refs.selectDocument.parentElement
                if (this.$v.$dirty && !this.$v.documentType.required) {
                    selectDocumentParent.classList.add('invalid')
                } else if (this.$v.$dirty && this.$v.documentType.required) {
                    selectDocumentParent.classList.remove('invalid')
                }

            },
            setDateOfBirth() {
                this.dateOfBirth = this.$refs.dateOfBirth.value
            },
            setDateOfIssue() {
                this.dateOfIssue = this.$refs.dateOfIssue.value
            },
            submitHandler() {

                if (this.$v.$invalid) {
                    this.$v.$touch()
                    let anime = this.$refs.selectCustomer.parentElement
                    if (this.$v.$dirty && !this.$v.customerGroup.required) {
                        anime.classList.add('invalid')
                    } else if (this.$v.$dirty && this.$v.customerGroup.required) {
                        anime.classList.remove('invalid')
                    }
                    let anime1 = this.$refs.selectDocument.parentElement
                    if (this.$v.$dirty && !this.$v.documentType.required) {
                        anime1.classList.add('invalid')
                    } else if (this.$v.$dirty && this.$v.documentType.required) {
                        anime1.classList.remove('invalid')
                    }
                    return
                } else {
                    M.toast({html: 'Клиент успешно создан!'})
                    let data = {
                        first_name: this.first_name,
                        last_name: this.last_name,
                        patronymic: this.patronymic,
                        dateOfBirth: this.dateOfBirth,
                        phone: this.phone,
                        sex: this.sex,
                        customerGroup: this.customerGroup,
                        doctor: this.doctor,
                        sendSms: this.sendSms,
                        index: this.index,
                        country: this.country,
                        region: this.region,
                        city: this.city,
                        street: this.street,
                        house: this.house,
                        documentType: this.documentType,
                        series: this.series,
                        number: this.number,
                        issuedBy: this.issuedBy,
                        dateOfIssue: this.dateOfIssue
                    }
                    console.log(data)
                }
            }
        },
        mounted() {

            M.Datepicker.init(this.$refs.dateOfBirth)
            M.CharacterCounter.init(this.$refs.phone)
            M.FormSelect.init(this.$refs.selectCustomer)
            M.FormSelect.init(this.$refs.selectDoctor)
            M.FormSelect.init(this.$refs.selectDocument)
            M.Datepicker.init(this.$refs.dateOfIssue)

        },
        destroyed() {

            let instanceDateOfBirth = M.Datepicker.getInstance(this.$refs.dateOfBirth);
            if (instanceDateOfBirth && instanceDateOfBirth.destroy) {
                instanceDateOfBirth.destroy()
            }

            let instanceDateOfIssue = M.Datepicker.getInstance(this.$refs.dateOfIssue);
            if (instanceDateOfIssue && instanceDateOfIssue.destroy) {
                instanceDateOfIssue.destroy()
            }

            let instancesFormSelectCustomer = M.FormSelect.init(this.$refs.customerGroup);
            if (instancesFormSelectCustomer && instancesFormSelectCustomer.destroy) {
                instancesFormSelectCustomer.destroy()
            }

            let instancesFormSelectDoctor = M.FormSelect.init(this.$refs.selectDoctor);
            if (instancesFormSelectDoctor && instancesFormSelectDoctor.destroy) {
                instancesFormSelectDoctor.destroy()
            }

            let instancesFormSelectDocument = M.FormSelect.init(this.$refs.selectDocument);
            if (instancesFormSelectDocument && instancesFormSelectDocument.destroy) {
                instancesFormSelectDocument.destroy()
            }
        },
        components: {}
    }
</script>

<style lang="scss" scoped>
    .home {
        .home__title {
            max-width: 800px;
            margin: 0 auto;
            text-align: center;
        }

        .wrapper {
            width: 80%;
            max-width: 400px;
            margin: 100px auto;
        }

        button {
            display: block;
            margin: 0 auto;
        }
    }


</style>
