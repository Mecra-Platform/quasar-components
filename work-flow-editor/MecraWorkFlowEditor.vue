<template>
  <h5>{{ flowData.title }}</h5>

  <q-timeline :color="flowData.color" :style="flowData.style">
    <q-timeline-entry
      v-for="flowStep in getSteps(flowData.id)"
      :key="flowStep.id"
      :title="flowStep.title"
      :icon="flowStep.icon"
      :color="flowStep.color"
      @click="getStepDetail(flowStep)"
    >
      <div>{{ flowStep.id }} {{ flowStep.body }}</div>
      <q-timeline v-if="lenSubSteps(flowStep.id) > 0" style="margin: 50px 0 0 33px">
        <q-timeline-entry
          v-for="first in getSteps(flowStep.id)"
          :key="first.id"
          :title="first.title"
          :icon="first.icon"
          :color="first.color"
          @click="getStepDetail(first)"
        >
          <div>{{ first.id }} {{ first.body }}</div>
          <q-timeline v-if="lenSubSteps(first.id) > 0" style="margin: 50px 0 0 33px">
            <q-timeline-entry
              v-for="second in getSteps(first.id)"
              :key="second.id"
              :title="second.title"
              :icon="second.icon"
              :color="second.color"
              @click="getStepDetail(second)"
            >
              <div>{{ second.id }} {{ second.body }}</div>
            </q-timeline-entry>
          </q-timeline>
        </q-timeline-entry>
      </q-timeline>
    </q-timeline-entry>
  </q-timeline>
</template>
<script>
import { defineComponent } from "vue";
export default defineComponent({
  props: {
    flowData: {
      type: Object,
      default: () => {},
    },
    getStepDetail: {
      type: Function,
      default: () => (step) => {
        console.log("getStepDetail callback function not configured", step);
      },
    },
  },
  setup(props) {
    const lenSubSteps = (parentId) => {
      const steps = props.flowData.steps.filter((step) => {
        return step.parentId === parentId;
      });
      return steps.length;
    };
    const getSteps = (parentId) => {
      return props.flowData.steps.filter((step) => {
        return step.parentId === parentId;
      });
    };
    return {
      getSteps,
      lenSubSteps,
    };
  },
});
</script>
