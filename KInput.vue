<template>
  <label class="block">
    <div v-if="label" :class="labelClasses">{{ label }}</div>
    <div :class="inputWrapperClasses">
      <span v-if="prefix" class="pr-2 pl-4">
        {{ prefix }}
      </span>
      <input
        class="text-textColor"
        v-model="model"
        :type="type"
        :class="inputClasses"
        :placeholder="placeholder"
        :readonly="readonly"
        :disabled="disabled"
      >
      <span v-if="suffix" class="pl-2 pr-4">
        {{ suffix }}
      </span>
    </div>
    <div v-if="hasError" :class="errorWrapperClasses">
      <div :class="errorMessageClasses">{{ errorMessage }}</div>
    </div>
  </label>
</template>

<script>
import { useVModel } from '@vueuse/core'
import {
  useErrorMessages,
  declareErrorMessagesProps
} from './utils/errorMessages'
import { declareInputProps } from './utils/inputs'

export default {
  props: {
    ...declareErrorMessagesProps(),
    ...declareInputProps(),
    modelValue: {
      type: [String, Number],
      default: ''
    },
    type: {
      type: String,
      default: 'text'
    },
    prefix: {
      type: String,
      default: ''
    },
    suffix: {
      type: String,
      default: ''
    }
  },
  setup(props, { emit }) {
    const model = useVModel(props, 'modelValue', emit)
    const { hasError, errorMessage } = useErrorMessages(props)
    return {
      model,
      hasError,
      errorMessage
    }
  },
  computed: {
    labelClasses() {
      const classes = ['text-[12px] font-semibold text-[#99B4E7] mb-[7px]']
      return classes
    },
    inputClasses() {
      const classes = [
        'bg-transparent border-none outline-none w-full font-medium placeholder:text-[#99B4E7] placeholder:font-medium'
      ]
      if (!this.prefix) {
        classes.push('pl-[16px]')
      }
      if (!this.suffix) {
        classes.push('pr-[16px]')
      }
      return classes
    },
    errorWrapperClasses() {
      const classes = ['']
      if (this.hasError) {
        classes.push('h-[16px]')
      } else {
        classes.push('h-[0px]')
      }
      return classes
    },
    errorMessageClasses() {
      const classes = ['text-error text-[12px] font-bold']
      return classes
    },
    inputWrapperClasses() {
      const classes = [
        'bg-white dark:bg-background h-[50px] border border-solid rounded-[4px] flex items-center font-medium'
      ]
      if (this.hasError) {
        classes.push('border-error')
      } else {
        classes.push('border-[#E7EFFF]')
      }
      return classes
    }
  }
}
</script>
