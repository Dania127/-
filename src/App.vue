<template>

  <div class="container mt-4">
    <div class="col-sm-5 mx-auto">       
        <h2 class="reg-title"><b>Форма подачи заявления в отдел сервиса и качества</b></h2>
      <form @submit.prevent="FormSent" novalidate>
        <div v-show="step === 1">

          <div v-if="regMessage" class="alert alert-success" role="alert">
             Форма успешно отправлена!
          </div>

          <div class="form-group">
              <label for="name">Ваш филиал</label>
          </div>
              
          <div class="form-group">
            <select class="form-select" aria-label="Выберете город">
              <option selected>Выберете город</option>
              <option value="Москва">Москва</option>
              <option value="Санкт-Петербург">Санкт-Петербург</option>
              <option value="Екатеринбург">Екатеринбург</option>
              <option value="Челябинск">Челябинск</option>
            </select>

            <div class="form-check">
            <input class="form-check-input" type="checkbox" value="" id="flexCheckDefault">
            <label class="form-check-label" for="flexCheckDefault">
              Онлайн
            </label>
            </div>
          </div>


          <div class="form-group">
              <label for="surname">Тема обращения</label>

              <div class="form-check">
              <input class="form-check-input" type="radio" name="flexRadioDefault" id="flexRadioDefault1">
              <label class="form-check-label" for="flexRadioDefault1">
                Недоволен качеством услуг
              </label>
            </div>
            <div class="form-check">
              <input class="form-check-input" type="radio" name="flexRadioDefault" id="flexRadioDefault2" checked>
              <label class="form-check-label" for="flexRadioDefault2">
                Расторжение договора
              </label>
              </div>
            <div class="form-check">
            <input class="form-check-input" type="radio" name="flexRadioDefault" id="flexRadioDefault1">
            <label class="form-check-label" for="flexRadioDefault1">
              Не приходит письмо активации на почту
            </label>
            </div>
          <div class="form-check">
            <input class="form-check-input" type="radio" name="flexRadioDefault" id="flexRadioDefault2" checked>
            <label class="form-check-label" for="flexRadioDefault2">
              Не работает личный кабинет
            </label>
          </div>

          <label for="other">Другое</label>
              <input @blur="$v.form.other.$touch()"
                     :class="status($v.form.other)"
                     v-model.trim="form.other"
                     type="text" class="form-control" id="other">

              <div class="invalid-feedback" v-if="!$v.form.other.required">{{ reqText }}</div>
              <div class="invalid-feedback" v-if="!$v.form.other.alpha">{{ alphaText }}</div>
          </div>

          <div class="form-group">
            <label for="problem">Описание проблемы</label>
              <input @blur="$v.form.problem.$touch()"
                     :class="status($v.form.problem)"
                     v-model.trim="form.problem"
                     type="text" class="form-control" id="problem">

              <div class="invalid-feedback" v-if="!$v.form.problem.required">{{ reqText }}</div>
              <div class="invalid-feedback" v-if="!$v.form.other.alpha">{{ alphaText }}</div>
          </div>

          <div class="form-group">
              <label for="other">Загрузка документов</label>

          <div class="mb-3">
            <input class="form-control form-control-sm" id="formFileSm" type="file">
            </div>
          </div>

          <button @click="step++" :disabled="disabledBtn1"
                  type="submit" class="btn btn-primary">Отправить</button>

        
        
        </div>  
      </form>
    </div>
  </div>
</template>

<script>
// const URL_ = 'https://60254fac36244d001797bfe8.mockapi.io/api/v1/send-form'
import { required } from 'vuelidate/lib/validators'


export default {
   data() {
     return {
        step: 1,
        regMessage: false,
        reqText: 'Поле обязательно для заполнения',
        form: {
          other: '',
          problem: '',
        }
     }
   },
   computed: {
    disabledBtn1() {
       return this.$v.form.other.$invalid || 
              this.$v.form.problem.$invalid  
    },
    disabledBtnFinish() {
       return this.$v.form.other.$invalid || 
              this.$v.form.problem.$invalid 
    }
   },
   methods: {
    status(validation) {
       return {
         'is-invalid': validation.$error,
         'error': validation.$error
       }
    },
    FormSent() {
        console.group()
          console.log('Форма успешно отправлена!')

        this.reset()
        
    },
    reset() {
        // сбросить шаг и показать сообщение об отправке
        this.step = 1;
        this.regMessage = true;

        
        setTimeout(() => {
          this.regMessage = false
        }, 3000)

        // сбросить все поля
        for (let input in this.form) {
            this.form[input] = ''
        }

        // сбросить валидацию
        this.$v.$reset()
    }
  },
  validations: {
      form: {
          problem: {
            required
          },
          other: {
            required       
          }
      }
  },
  created() {
      for (let i = this.yearEnd; i >= 1800; i--) this.years.push(i)
  }
}
</script>

<style>
body {
  background-color: white;
}

form {
  background-color: #fffefe;
  padding: 25px;
  border: 1px solid rgb(216, 211, 211);
  border-radius: 1px;
  box-shadow: 10px 10px 4px -300px rgba(0, 0, 0, 0.75);
}

.error {
  background-color: #fdd;
}

.reg-title {
  color: #5d5d5d;
  font-size: 24px;
  margin-bottom: 18px;
  padding-left: 20px;
}

.slide-fade-enter-active {
  transition: all 0.3s ease;
}

.slide-fade-enter {
  transform: translateX(10px);
  opacity: 0;
}
</style>
