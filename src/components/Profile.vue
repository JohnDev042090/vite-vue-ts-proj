    
<template v-slot:right>
  <ProfileJsonForm>
    
  </ProfileJsonForm>
    <div style="display:flex; justify-content: space-evenly; flex-wrap: wrap;">
        <Form>
          <FormElement style="border: 1px solid #018786; padding: 40px; box-shadow: 1px 2px 3px #03DAC6; border-radius: 5px;">
              <avatar style="background-color: #018786;" :rounded="'full'" :type="'icon'" :themeColor="'secondary'" size="large">
                  <span class="k-icon k-i-user" style="font-size: xx-large;" />
              </avatar>
    
              <h2 style="color: #03DAC6;">Profile</h2>
              <hr> 
              <br />
              
              <Field :id="'firstname'" :name="'firstname'" :component="'myTemplate'" v-model="firstname" :label="'First Name'" :type="'text'" :placeholder="'Enter your first name'" :iconName="'user'">
                  <template v-slot:myTemplate="{props}">
                  <forminput
                      v-bind="props"
                      @change="inputFirstName"
                      @blur="inputFirstName"
                      @focus="props.onFocus"
                    />
                  </template>
              </Field>
              
              <Field :id="'lastname'" :name="'lastname'" :component="'myTemplate'" v-bind:lastname="lastname" :label="'Last Name'" :type="'text'" :placeholder="'Enter your last name'"  :iconName="'user'">
                  <template v-slot:myTemplate="{props}">
                    <forminput
                      v-bind="props"
                      @change="inputLastName"
                      @blur="inputLastName"
                      @focus="props.onFocus"
                    />
                  </template>
              </Field>
              
              <Field :id="'bday'" :name="'bday'" :component="'myTemplate'" v-model.lazy="bday" :label="'Birthday'" :type="'date'" :placeholder="'Enter your birthdate'"  :iconName="'calendar'">
                  <template v-slot:myTemplate="{props}">
                  <forminput
                      v-bind="props"
                      @change="inputBirthday"
                      @blur="props.onBlur"
                      @focus="props.onFocus"
                    />
                  </template>
              </Field>
    
              <Field :style="'display:' + _ishidden" :id="'email'" :name="'email'" :component="'myTemplate'" v-bind:email="email" :label="'Email'" :validator="emailValidator" :type="'email'" :placeholder="'youremail@example.com'" :showValidationIcon="_showValidationIcon"  :iconName="'email'">
                  <template v-slot:myTemplate="{props}">
                  <forminput
                      v-bind="props"
                      @change="props.onChange"
                      @blur="props.onBlur"
                      @focus="props.onFocus"
                    />
                  </template>
              </Field>
            <br />
            <br />
            <Button style="width: 100px; border-color: #018786; background-color: #018786; color: #eee;" :icon="'arrow-chevron-right'" :fill-mode="'outline'" :themeColor="'base'" :rounded="'full'" :disabled="_isBtnDisabled" @click="submitForm">Next</Button>
            
          </FormElement>
        </Form>
      </div>

</template>
    
  <script lang="ts">
  import '@progress/kendo-licensing';
  import '@progress/kendo-svg-icons';
  import { Form, FormElement, FieldWrapper, Field } from '@progress/kendo-vue-form';
  import { Button } from "@progress/kendo-vue-buttons";
  import { DateInput, DatePicker } from "@progress/kendo-vue-dateinputs";
  import { Input } from '@progress/kendo-vue-inputs';
  import { Error, Hint, Label, FloatingLabel } from "@progress/kendo-vue-labels";
  import { Avatar } from "@progress/kendo-vue-layout";

  import FormInput from "./LabelFormInput.vue";
  import ProfileJsonForm from "./ProfileJsonForm.vue";

  //3. Should validate the email using a regular expression
  const emailRegex = new RegExp(/\S+@\S+\.\S+/);
  const emailValidator = (value: string) => emailRegex.test(value) ? "" : "Please enter a valid email.";
  const _showValidationIcon = false;
  const _isBtnDisabled = true;
  const _ishidden = 'none';

  export default {
    components: {
        Form,
        FormElement,
        Button,
        DateInput,
        DatePicker,
        Input,
        FieldWrapper,
        Field,
        Hint,
        Error,
        Label,
        FloatingLabel,
        'forminput': FormInput,
        Avatar,
        ProfileJsonForm
    },
    emits: {
        change: null,
        blur: null,
        focus: null
    },
    data() {
      return {
        firstname: '',
        lastname: '',
        email: '',
        bday: '',
        bdate: new Date,
        emailValidator: emailValidator,
        _showValidationIcon: _showValidationIcon,
        _isBtnDisabled: _isBtnDisabled,
        _ishidden: _ishidden,
        minDate: '2000-01-01',
        maxDate: new Date().toISOString().split('T')[0]
      };
    },
    inject: {
        kendoForm: { default: {} }
    },
    methods: {
        handleChange(e: any){
            this.$emit('change', e);
        },
        handleBlur(e: any){
            this.$emit('blur', e);
        },
        handleFocus(e: any){
            this.$emit('focus', e);
        },
        submitForm() {
            console.log('Form submitted');
            this._showValidationIcon = true;
        },
        calculateAge() {
          const birthdate = new Date(this.bday);
          //const birthdate = new Date(this.bdate);
          const today = new Date();
          let age = today.getFullYear() - birthdate.getFullYear();
          const monthDiff = today.getMonth() - birthdate.getMonth();
          // if birthdate month is less than current month
          // OR if birthdate month is equal to current month but the birthdate day is grater than current day
          if (monthDiff < 0 || (monthDiff === 0 && today.getDate() < birthdate.getDate())) { age--; }
          if(age < 18) {
            this._ishidden = 'none';
            this._isBtnDisabled = true;
          }
          else {
            this._ishidden = '';
            this._isBtnDisabled = false;
          }
        },
        inputFirstName(val: any) {
          this.firstname = val.target.value;
        },
        inputLastName(val: any) {
          this.lastname = val.target.value;
        },
        inputEmail(val: any) {
          this.email = val.target.value;
        },
        inputBirthday(val: any) {
          this.bday = val.target.value;
          this.bdate = val.target.value? new Date(val.target.value) : new Date();
          this.calculateAge();
        }
    }
  };
  </script>
  