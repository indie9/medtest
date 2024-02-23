<template>
  <div class="form">
    <div class="form__header">
      <h2>Форма создания Клиента</h2>
    </div>

    <form @submit.prevent="submitForm">
      <div class="form__group">
        <Label label="Фамилия" :error="errors['lastName']" necessarily>
          <Input v-model="formData.lastName" label="Фамилия" placeholder="Галанов" :error="errors['lastName']"
            necessarily />
        </Label>
      </div>

      <div class="form__group">
        <Label label="Имя" :error="errors['firstName']" necessarily>
          <Input v-model="formData.firstName" placeholder="Максим" necessarily :error="errors['firstName']" />
        </Label>
      </div>

      <div class="form__group">
        <Label label="Отчество" :error="errors['patronymic']">
          <Input v-model="formData.patronymic" placeholder="Эрнстович" :error="errors['patronymic']" />
        </Label>
      </div>

      <div class="form__group">
        <Label label="Дата рождения" :error="errors['dateOfBirth']" necessarily>
          <Input v-model="formData.dateOfBirth" typeInput="date" :error="errors['dateOfBirth']" />
        </Label>
      </div>

      <div class="form__group">
        <Label label="Номер телефона" :error="errors['phoneNumber']" necessarily>
          <Input v-model="formData.phoneNumber" placeholder="89213456789" :error="errors['phoneNumber']" />
        </Label>
      </div>

      <div class="form__group">
        <Label label="Пол" :error="errors['gender']">
          <Select :options="genderArray" v-model="formData.gender" :isError="$v.formData.gender.$error" />
        </Label>
      </div>

      <div class="form__group">
        <Label label="Группа клиентов" :error="errors['clientGroup']" necessarily>
          <MultiSelect :options="customerGroup" v-model="formData.clientGroup"
            :isError="$v.formData.clientGroup.$error" />
        </Label>
      </div>

      <div class="form__group">
        <Label label="Лечащий врач" :error="errors['attendingDoctor']" necessarily>
          <Select v-model="formData.attendingDoctor" :options="doctorsArray" label="Лечащий врач" necessarily
            :isError="$v.formData.attendingDoctor.$error" />
        </Label>
      </div>

      <div class="form__group">
        <Checkbox v-model="formData.noSMS" id="sms" label="Не отправлять СМС" />
      </div>

      <div class="form__address-group">
        <h3>Адрес</h3>
        <div class="form__group">
          <Label label="Индекс" :error="errors['index']">
            <Input v-model="formData.index" placeholder="164500" :error="errors['index']" />
          </Label>
        </div>
        <div class="form__group">
          <Label label="Страна" :error="errors['country']">
            <Input v-model="formData.country" label="Страна" placeholder="Россия" :error="errors['country']" />
          </Label>
        </div>

        <div class="form__group">
          <Label label="Область" :error="errors['region']">
            <Input v-model="formData.region" label="Область" placeholder="Ленинградская" :error="errors['region']" />
          </Label>
        </div>

        <div class="form__group">
          <Label label="Город" :error="errors['city']" necessarily>
            <Input v-model="formData.city" label="Город" placeholder="Санкт-Петербург" :error="errors['city']" />
          </Label>
        </div>

        <div class="form__group">
          <Label label="Улица" :error="errors['street']">
            <Input v-model="formData.street" label="Улица" placeholder="Московский проспект" :error="errors['street']" />
          </Label>
        </div>

        <div class="form__group">
          <Label label="Дом" :error="errors['house']">
            <Input v-model="formData.house" label="Дом" placeholder="24а" :error="errors['house']" />
          </Label>
        </div>
      </div>

      <div class="form__passport-group">
        <h3>Паспорт</h3>
        <div class="form__group">
          <Label label="Тип документа" :error="errors['type']" necessarily>
            <Select :options="documentsArray" v-model="formData.type" :isError="$v.formData.type.$error" />
          </Label>
        </div>
        <div class="form__group">
          <Label label="Серия" :error="errors['serial']">
            <Input v-model="formData.serial" label="Серия" placeholder="1116" :error="errors['serial']" />
          </Label>
        </div>

        <div class="form__group">
          <Label label="Номер" :error="errors['number']" necessarily>
            <Input v-model="formData.number" placeholder="40234090" :error="errors['number']" />
          </Label>
        </div>

        <div class="form__group">
          <Label label="Кем выдан" :error="errors['issuedBy']">
            <Input v-model="formData.issuedBy" placeholder="ОТДЕЛОМ УФМС" :error="errors['issuedBy']" />
          </Label>
        </div>

        <div class="form__group">
          <Label label="Дата выдачи" :error="errors['issueDate']" necessarily>
            <Input v-model="formData.issueDate" typeInput="date" :error="errors['issueDate']" />
          </Label>
        </div>
      </div>

      <div class="form__button">
        <Button type="submit">Создать Клиента</Button>
      </div>
    </form>
    <FormResult v-model="openForm" />
  </div>
</template>

<script>
import Button from "@/components/Button/Button.vue";
import Input from "@/components/Input/Input.vue";
import Select from "@/components/Select/Select.vue";
import MultiSelect from "@/components/MultiSelect/MultiSelect.vue";
import Checkbox from "@/components/Checkbox/Checkbox.vue";
import Label from "@/components/LabelInput/LabelInput.vue";
import FormResult from "@/components/FormResult/FormResult.vue";

import {
  required,
  numeric,
  minLength,
  maxLength,
} from "vuelidate/lib/validators";

export default {
  components: {
    Button,
    Input,
    Select,
    MultiSelect,
    Checkbox,
    Label,
    FormResult,
  },
  data() {
  const emptyData = {
    lastName: "",
    firstName: "",
    patronymic: "",
    dateOfBirth: "",
    phoneNumber: "",
    gender: "",
    clientGroup: [],
    attendingDoctor: "",
    noSMS: false,

    index: "",
    country: "",
    region: "",
    city: "",
    street: "",
    house: "",

    type: "",
    serial: "",
    number: "",
    issuedBy: "",
    issueDate: "",
  };

  return {
    emptyData: emptyData,
    formData: { ...emptyData },
    doctorsArray: ["Иванов", "Захаров", "Чернышева"],
    documentsArray: [
      "Паспорт",
      "Свидетельство о рождении",
      "Водительское удостоверение",
    ],
    customerGroup: ["VIP", "Проблемные", "ОМС"],
    genderArray: ["Мужской", "Женский"],
    errors: {},
    openForm: false,
  };
},
  validations: {
    formData: {
      lastName: {
        required,
        russianLetters: (value) => /^[а-яА-Я\s]+$/.test(value),
      },
      firstName: {
        required,
        russianLetters: (value) => /^[а-яА-Я\s]+$/.test(value),
      },
      patronymic: {
        russianLetters: (value) => /^$|^[\sа-яА-Я-]+$/.test(value),
      },
      dateOfBirth: {
        required,
      },
      phoneNumber: {
        required,
        validPhoneNumber: (value) => /^\+?\d{11}$/.test(value),
      },
      gender: {
      },
      clientGroup: {
        lengthArr: (value) => value.length > 0,
      },
      attendingDoctor: {
        required,
      },
      noSMS: {},
      index: {
        numeric,
        minLength: minLength(4),
        maxLength: maxLength(8),
      },
      country: {
        russianLetters: (value) => /^$|^[\sа-яА-Я-]+$/.test(value),
      },
      region: {
        russianLetters: (value) => /^$|^[\sа-яА-Я-]+$/.test(value),
      },
      city: {
        required,
        russianLetters: (value) => /^[а-яА-Я\s\-]+$/.test(value),
      },
      street: {
        russianLetters: (value) => /^$|^[\sа-яА-Я-]+$/.test(value),
      },
      house: {
        numeric,
        maxLength: maxLength(6),
      },
      type: {
        required,
      },
      serial: {
        numeric,
        minLength: minLength(4),
        maxLength: maxLength(8),
      },
      number: {
        required,
        numeric,
        minLength: minLength(4),
        maxLength: maxLength(14),
      },
      issuedBy: {
        russianLetters: (value) => /^$|^[\sа-яА-Я-]+$/.test(value),
      },
      issueDate: {
        required,
      },
    },
  },
  methods: {
    submitForm() {
      this.$v.formData.$touch();
      if (!this.$v.formData.$invalid) {
        console.log(this.formData);
        this.openForm = true;
        this.errors = {};
        this.resetForm();
      } else {
        this.errors = this.getErrors();
      }
    },
    resetForm() {
      this.formData = this.emptyData;
      this.errors = {};
      this.$v.$reset();
    },
    getErrors() {
      let errors = {};
      for (const key in this.$v.formData.$params) {
        if (this.$v.formData[key]?.$error) {
          errors[key] = `Поле должно быть заполнено`;

          const errorMessages = [];
          if (
            this.$v.formData[key].required !== undefined &&
            !this.$v.formData[key].required
          ) {
            errors[key] = "Поле должно быть заполнено";
            continue;
          }
          if (
            this.$v.formData[key].russianLetters !== undefined &&
            !this.$v.formData[key].russianLetters
          ) {
            errors[key] = `Поле должно содержать кириллицу или '-'`;
            continue;
          }
          if (
            this.$v.formData[key].numeric !== undefined &&
            !this.$v.formData[key].numeric
          ) {
            errors[key] = "Поле должно быть числом";
            continue;
          }
          if (
            (this.$v.formData[key].minLength !== undefined &&
              !this.$v.formData[key].minLength) ||
            (this.$v.formData[key].maxLength !== undefined &&
              !this.$v.formData[key].maxLength)
          ) {
            errors[
              key
            ] = `Число должно содержать от ${this.$v.formData[key].$params.minLength.min} до ${this.$v.formData[key].$params.maxLength.max} символов`;
            continue;
          }
          if (
            this.$v.formData[key].lengthArr !== undefined &&
            !this.$v.formData[key].lengthArr
          ) {
            errors[key] = `Выберите хотя бы один элемент`;
            continue;
          }
          if (
            this.$v.formData[key].validPhoneNumber !== undefined &&
            !this.$v.formData[key].validPhoneNumber
          ) {
            errors[key] = `Введите корректный номер телефона`;
            continue;
          }
        }
      }
      return errors;
    },
  },
};
</script>

<style lang="sass" scoped>
@import "index.sass"
</style>
