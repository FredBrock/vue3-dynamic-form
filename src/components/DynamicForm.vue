

<script >
import {
  ElButton,
  ElForm,
  ElInput,
  ElFormItem,
  ElSwitch,
  ElSelect,
  ElOption,
  ElDatePicker,
  ElCheckboxGroup,
  ElCheckbox,
  ElRadioGroup,
  ElRadio,
  ElDialog,
  ElMessage,
} from "element-plus";
import { reactive, h, ref, toRefs, unref } from "vue";
export default {
  props: ["modelValue", "fields"],
  emits: ["update:modelValue"],
  components: {
    ElButton,
  },
  setup(props, { emit }) {
    const form = reactive({
      name: "",
      region: "",
      date1: "",
      date2: "",
      delivery: false,
      type: [],
      resource: "",
      desc: "",
    });

    let layouts = {
      input: h(ElInput, {}),
      switch: h(ElSwitch),
      textarea: h(ElInput, {
        type: "textarea",
      }),
      dateRange: h(ElDatePicker, {
        type: "daterange",
      }),
      radio: h(ElRadioGroup, {}, () => [
        h(ElRadio, { label: "是", value: "1" }),
        h(ElRadio, { label: "否", value: "0" }),
      ]),
      checkboxes: h(
        ElCheckboxGroup,
        {
          options: [
            { label: "杭州", value: "a" },
            { label: "武汉", value: "b" },
            { label: "湖州", value: "c" },
            { label: "贵阳", value: "d" },
          ],
        },
        () => [
          h(ElCheckbox, { label: "杭州", value: "a" }),
          h(ElCheckbox, { label: "武汉", value: "b" }),
          h(ElCheckbox, { label: "湖州", value: "c" }),
          h(ElCheckbox, { label: "深圳", value: "d" }),
        ]
      ),
      select: h(
        ElSelect,
        {
          placeholder: "please select your zone",
          props: {
            options: [
              { label: "早", value: "a" },
              { label: "中", value: "b" },
              { label: "晚", value: "c" },
            ],
          },
        },
        () => [
          h(ElOption, { label: "早", value: "a" }),
          h(ElOption, { label: "中", value: "b" }),
          h(ElOption, { label: "晚", value: "c" }),
        ]
      ),
    };

    let fromProps = ref({
      modelValue: props.modelValue,
      "onUpdate:modelValue": (value) => emit("update:modelValue", value),
      labelWidth: "auto",
      "max-width": "600px",
    });

    function renderField(field) {
      let layout = layouts[field["widget"]];
      if (layout) {
        return h(layout, {
          modelValue: form[field["prop"]],
          "onUpdate:modelValue": (value) => {
            form[field["prop"]] = value;
            emit("update:modelValue", form);
          },
        });
      }
    }
    function renderFields() {
      let fields = reactive(props.fields);
      let children = fields.map((field) => {
        return h(ElFormItem, { label: field.label }, () => [
          renderField(field),
        ]);
      });

      return children;
    }
    function buildForm() {
      return h(ElForm, fromProps.value, () => [
        renderFields(),
        h(
          ElButton,
          {
            onClick() {
              alert(JSON.stringify(form, null, 2));
            },
            type: "primary",
          },
          () => "Submit"
        ),
      ]);
    }
    return buildForm;
  },
};
</script>