<template>
    <kfieldwrapper>
        <klabel :editor-id="id" :editor-valid="valid" :disabled="disabled" :optional="optional">
            {{ label }}
        </klabel>
        <kinput
            :valid="valid"
            :id="id"
            :type="type"
            :value="value"
            :disabled="disabled"
            :placeholder="placeholder"
            :show-validation-icon="showValidationIcon"
            :style="{ width: '300px' }"
            :icon-name="iconName"
            :maxlength="'50'"
            :min="'1900-01-01'"
            :max="new Date().toISOString().split('T')[0]"
            @input="handleChange"
            @blur="handleBlur"
            @focus="handleFocus"
            />
        <kerror v-if="showValidationMessage">
            {{validationMessage}}
        </kerror>
        <khint v-else>{{hint}}</khint>
    </kfieldwrapper> 
</template>

<script lang="ts">
import { FieldWrapper } from "@progress/kendo-vue-form";
import { Error, Hint, Label } from "@progress/kendo-vue-labels";
import { Input } from "@progress/kendo-vue-inputs";

export default {
    props: {
        optional: Boolean,
        disabled: Boolean,
        placeholder: String,
        touched: Boolean,
        label: String,
        validationMessage: String,
        hint: String,
        id: String,
        valid: Boolean,
        value: {
            type: String,
            default: ''
        },
        type: {
            type: String,
            default: 'text'
        },
        showValidationIcon: {
            type: Boolean,
            default: false
        },
        iconName: {
            type: String,
            default: ''
        }
    },
    components: {
      kfieldwrapper: FieldWrapper,
      kerror: Error,
      khint: Hint,
      klabel: Label,
      kinput: Input
    },
    computed: {
        showValidationMessage() {
            return this.$props.touched && this.$props.validationMessage;
        },
        showHint() {
            return !this.showValidationMessage && this.$props.hint;
        },
        hintId() {
            return this.showHint ? `${this.$props.id}_hint` : "";
        },
        errorId() {
            return this.showValidationMessage ? `${this.$props.id}_error` : "";
        }
    },
    emits: {
        change: null,
        blur: null,
        focus: null
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
        }
    }
}
</script>
