<template>
  <div class="BInput">
    <div class="BInput__wrapper">
      <template v-if="mask">
        <input
          @input="this.errors = null"
          v-on:keyup.enter="emitAnswer"
          class="BInput__field"
          :type="type"
          v-model="inputdata"
          v-mask="mask"
          masked="true"
          :placeholder="placeholder"
        />
      </template>
      <template v-else>
        <input
          @input="this.errors = null"
          maxlength="40"
          v-on:keyup.enter="emitAnswer"
          class="BInput__field"
          :type="type"
          v-model="inputdata"
          :placeholder="placeholder"
        />
      </template>
    </div>
    <BFormErrors :errors="errors"></BFormErrors>
    <BFormBtn
      :disabled="errors && errors.length"
      @click="emitAnswer"
    ></BFormBtn>
  </div>
</template>
<script>
import BIcon from "./BIcon.vue";
import BFormBtn from './BFormBtn.vue';
import BFormErrors from './BFormErrors.vue';
import validationMixin from '../../mixins/validations'
export default {
  mixins: [validationMixin],
  methods:{
    emitAnswer(){
      if (this.errors && this.errors.length) return
      this.errors = this.validateForm(this.inputdata, this.type);
      if(this.errors.formIsValid) {
        this.$emit('set-answer', this.inputdata)
      }
    },
  },
  props: {
    placeholder: {
      type: String,
      default: ''
    },
    type: {
      type: String,
      default: 'text'
    },
    mask: {
      type: String,
      default: ''
    },
  },
  components: {
    BFormErrors,
    BFormBtn,
    BIcon,
  },
  name: "BInput",
  data() {
    return {
      inputdata: "",
    };
  },
};
</script>