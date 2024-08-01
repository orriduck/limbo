注意`checkedSnowflakeOutput`这个变量的变化过程

Ref. from [[Vue component rendering]]

**Pseudocode:**
1. **Component Initialization:**
   - The `OutputsForm` component is initialized with its props and data.
   - The `checkedSnowflakeOutput` prop is passed from the parent component.

2. **Data and Props Setup:**
   - The `checkedSnowflakeOutput` prop is defined with a default value of `false`.
   - The `data` function initializes the `outputTypes` array with `jobRunOutputType.S3`.

3. **Created Lifecycle Hook:**
   - The `created` lifecycle hook is called immediately after the component is created.
   - Inside the `created` hook, the component emits an event `update:checkedSnowflakeOutput` with the initial value of `checkedSnowflakeOutput`.

4. **Parent Component Handling:**
   - The parent component listens for the `update:checkedSnowflakeOutput` event.
   - The parent component updates its own state based on the emitted value.

5. **Template Rendering:**
   - The template uses `v-model` to bind `checkedSnowflakeOutput` to the checkbox.
   - The initial value of `checkedSnowflakeOutput` is reflected in the checkbox state.

**Code:**
```vue
<template>
  <b-container fluid>
    <b-checkbox
        v-if="showSnowflakeOutputCheckbox"
        v-model="checkedSnowflakeOutput"
        v-b-tooltip:top="'Copy the Job Run Output to Snowflake'"
        title="Snowflake Output"
    />
    <div>
      If your job run succeed, the output will be stored in the following locations:
    </div>
    <b-row
        v-if="outputTypes.includes(jobRunOutputType.S3)"
        class="d-flex gap-0.5"
    >
      <b-tag
          variant="warning"
          :no-remove="true"
      >
        AWS S3
      </b-tag>
      <div>
        Your Job's output will be stored in S3
      </div>
    </b-row>
    <b-row
        v-if="outputTypes.includes(jobRunOutputType.SNOWFLAKE)"
        class="d-flex gap-0.5"
    >
      <b-tag
          variant="warning"
          :no-remove="true"
      >
        Snowflake
      </b-tag>
      <div>
        Your Job's output will be stored in Snowflake
      </div>
    </b-row>
  </b-container>
</template>

<script>
import jobRunOutputType from '@/JobRunOutputType';

export default {
  name: 'OutputsForm',
  props: {
    projectId: {
      type: [Number, String],
      required: false,
      default: undefined,
    },
    blueprintId: {
      type: [Number, String],
      required: false,
      default: undefined,
    },
    showSnowflakeOutputCheckbox: {
      type: Boolean,
      default: false,
    },
    checkedSnowflakeOutput: {
      type: Boolean,
      default: false,
      required: false,
    },
  },
  data: () => ({
    outputTypes: [jobRunOutputType.S3],
  }),
  computed: {
    jobRunOutputType() {
      return jobRunOutputType;
    },
  },
  watch: {
    checkedSnowflakeOutput: {
      immediate: true,
      handler(value) {
        this.$emit('update:checkedSnowflakeOutput', value);
      },
    },
  },
  created() {
    this.$emit('update:checkedSnowflakeOutput', this.checkedSnowflakeOutput);
  },
};
</script>
```

外部call这个组件
```vue
<template>
  <div>
    <!-- Other parent component content -->
    <OutputsForm
      :projectId="projectId"
      :blueprintId="blueprintId"
      :showSnowflakeOutputCheckbox="showSnowflakeOutputCheckbox"
      v-model:checkedSnowflakeOutput="checkedSnowflakeOutput"
    />
  </div>
</template>

<script>
import OutputsForm from '@/views/forms/shared/OutputsForm.vue';

export default {
  name: 'ParentComponent',
  components: {
    OutputsForm,
  },
  data() {
    return {
      projectId: 123,
      blueprintId: 456,
      showSnowflakeOutputCheckbox: true,
      checkedSnowflakeOutput: false,
    };
  },
};
</script>
```