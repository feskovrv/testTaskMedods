<template>
  <form @submit.prevent="checkForm">
    <fieldset>
      <legend>Личные данные</legend>
      <label>Фамилия<form-input v-model="formData.lastName" type="text" placeholder="Иванов" />
      <span class="warning" v-if="checkField('lastName', 'required')">Поле обязательно для заполнения</span>
      </label>
      <label>Имя<form-input v-model="formData.firstName" type="text" placeholder="Иван" />
      <span class="warning" v-if="checkField('firstName', 'required')">Поле обязательно для заполнения</span>
      </label>
      <label>Отчество<form-input v-model="formData.secondName" type="text" placeholder="Иванович" /></label>
      <label>Дата рождения<form-input v-model="formData.birthDate" type="date"/>
      <span class="warning" v-if="checkField('birthDate', 'required')">Поле обязательно для заполнения</span>
      </label>
      <label>Номер телефона<form-input v-model="formData.phoneNumber" type="text" placeholder="79871234567" />
      <span class="warning" v-if="checkField('phoneNumber', 'required')">Поле обязательно для заполнения</span>
      <span class="warning" v-else-if="checkField('phoneNumber', 'phoneFormat')">Номер должен быть в формате 79876543210</span>
      </label>
      <label>Пол<form-radio v-model="formData.gender" :values="genderValues" name="gender" /></label>
      <label>Группа клиентов<form-select :options="groups" v-model="formData.group" />
      <span class="warning" v-if="checkField('group', 'required')">Поле обязательно для заполнения</span>
      </label>
      <label>Лечащий врач<form-select :options="doctors" v-model="formData.doctor" /></label>
      <label>Не присылать СМС<form-input v-model="formData.noSMS" type="checkbox"/></label>
    </fieldset>
    <fieldset>
      <legend>Адрес</legend>
      <label>Индекс<form-input v-model="formData.postIndex" type="text" placeholder="295000" /></label>
      <label>Страна<form-input v-model="formData.country" type="text" placeholder="Россия" /></label>
      <label>Область<form-input v-model="formData.region" type="text" placeholder="Московская" /></label>
      <label>Город<form-input v-model="formData.city" type="text" placeholder="Москва" />
      <span class="warning" v-if="checkField('city', 'required')">Поле обязательно для заполнения</span>
      </label>
      <label>Улица<form-input v-model="formData.street" type="text" placeholder="Ленина" /></label>
      <label>Дом<form-input v-model="formData.house" type="text" placeholder="35" /></label>
    </fieldset>
    <fieldset>
      <legend>Документ</legend>
      <label>Тип документа<form-select :options="documents" v-model="formData.document" />
      <span class="warning" v-if="checkField('document', 'required')">Поле обязательно для заполнения</span>
      </label>
      <label>Серия<form-input v-model="formData.docSeria" type="text" placeholder="11 22" /></label>
      <label>Номер<form-input v-model="formData.docNumber" type="text" placeholder="123456" /></label>
      <label>Кем выдан<form-input v-model="formData.docIssuedBy" type="text" placeholder="Федеральной миграционной службой" /></label>
      <label>Дата выдачи<form-input v-model="formData.docIssuedDate" type="date" />
      <span class="warning" v-if="checkField('docIssuedDate', 'required')">Поле обязательно для заполнения</span>
      </label>
    </fieldset>
    <input type="submit" value="ПОДТВЕРДИТЬ">
    <hr />
    <span class="success" v-if="isFormValid">Новый клиент успешно создан</span>
  </form>
</template>

<script>
import FormInput from './components/FormInput.vue'
import FormSelect from './components/FormSelect.vue'
import FormRadio from './components/FormRadio.vue'
import { validationMixin } from 'vuelidate'
import { required } from 'vuelidate/lib/validators'

export default {
  name: 'App',
  mixins: [validationMixin],
  components: {
    FormInput,
    FormSelect,
    FormRadio
  },
  data() {return {
      formData: {
        lastName: "",
        firstName: "",
        secondName: "",
        birthDate: "",
        phoneNumber: "",
        gender: "",
        group: "",
        doctor: "",
        noSMS: true,
        postIndex: "",
        country: "",
        region: "",
        city: "",
        street: "",
        house: "",
        document: "",
        docSeria: "",
        docNumber: "",
        docIssuedBy: "",
        docIssuedDate: ""
      },
      groups: ["VIP","Проблемные","ОМС"],
      genderValues: ["Муж", "Жен"],
      doctors: ["Иванов","Захаров","Чернышева"],
      documents: ["Паспорт","Свидетельство о рождении","Водительское удостоверение"],
      isFormValid: false
    }
  },
  methods: {
    checkForm(event){
      this.$v.$touch()
      this.successReport()
      this.successReport() && event.target.reset();
    },
    checkField(field, validator){
      return this.$v.$dirty && !this.$v.formData[field][validator]
    },
    successReport(){
      console.log("Success report")
      this.isFormValid = this.$v.$anyDirty && !this.$v.$error ? true : false
      return this.$v.$anyDirty && !this.$v.$error
    }
  },
  validations: {
    formData: {
      lastName: {required},
      firstName: {required},
      birthDate: {required},
      phoneNumber: {
        required,
        phoneFormat(val){
          return val.match(/^7\d{10}$/) !== null ? true : false
        }},
      group: {required},
      city: {required},
      document: {required},
      docIssuedDate: {required}
    }
  }
}
</script>

<style lang="sass">
  @import url('https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,100;0,300;0,400;0,500;0,700;0,900;1,100;1,300;1,400;1,500;1,700;1,900&display=swap')

  *
    box-sizing: border-box
    font-family: 'Roboto', sans-serif

  form
    display: flex
    flex-direction: column
    width: 40vw
    margin: 20px auto

  fieldset
    border: 1px solid rgba(0, 0, 0, 0.3)

  label
    font-size: 10px
    margin-bottom: -10px

  [type="submit"]
    width: fit-content
    align-self: flex-end
    margin: 15px
    padding: 10px 20px
    background: #50c150
    border-radius: 10px
    border: none
    

  .warning
    display: inline-block
    width: 100%
    color: red
    font-size: 10px
    transform: translateY(-7px)

  .success
    text-align: center
    color: green
    font-size: 18px  

</style>
