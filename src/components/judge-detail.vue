<template>
  <div class="mb-3" style="width: 300px">
    <input
      type="text"
      class="form-control"
      v-model="clonedJudge.name"
      style="width: 300px"
    />
    <button
      class="btn btn-success btn-sm mt-1"
      v-on:click="saveJudge"
      title="Save Judge"
    >
      <i class="fa fa-check fa-xs"> Save</i>
    </button>
    <button
      class="btn btn-danger btn-sm mt-1 ml-1"
      v-on:click="cancelJudge"
      title="Cancel Save Judge"
    >
      <i class="fa fa-check fa-xs"> Cancel</i>
    </button>
  </div>
</template>

<script>
export default {
  name: "JudgeDetail",
  props: {
    judge: {
      type: Object,
      default: () => {},
    },
  },
  data() {
    return {
      clonedJudge: { ...this.judge },
    };
  },
  methods: {
    cancelJudge() {
      this.$emit('cancel');
    },
    saveJudge() {
      this.$emit('save', this.clonedJudge);
    },
  },
};
</script>
