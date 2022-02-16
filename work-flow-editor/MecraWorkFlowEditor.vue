<template>
  <h5>{{ flowData.title }}</h5>

  <q-timeline :color="flowData.color" :style="flowData.style">
    <q-timeline-entry
      v-for="flowStep in getSteps(flowData.id)"
      :key="flowStep.id"
      :title="flowStep.title"
      :icon="flowStep.icon"
      :color="flowStep.color"
      @click="getStepExtraDetails"
    >
      <div>{{ flowStep.id }} {{ flowStep.body }}</div>
      <q-timeline v-if="lenSubSteps(flowStep.id) > 0" style="margin: 50px 0 0 33px">
        <q-timeline-entry
          v-for="first in getSteps(flowStep.id)"
          :key="first.id"
          :title="first.title"
          :icon="first.icon"
          :color="first.color"
          @click="getStepExtraDetails"
        >
          <div>{{ first.id }} {{ first.body }}</div>
          <q-timeline v-if="lenSubSteps(first.id) > 0" style="margin: 50px 0 0 33px">
            <q-timeline-entry
              v-for="second in getSteps(first.id)"
              :key="second.id"
              :title="second.title"
              :icon="second.icon"
              :color="second.color"
              @click="getStepExtraDetails"
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
  },
  setup(props) {
    console.log(props);
    const getStepExtraDetails = (step) => {
      console.log("getStepExtraDetails", step);
    };
    const lenSubSteps = (parentId) => {
      const steps = props.flowData.steps.filter(function (step) {
        return step.parentId === parentId;
      });
      return steps.length;
    };
    const getSteps = (parentId) => {
      console.log("getSteps", parentId);
      return props.flowData.steps.filter(function (step) {
        return step.parentId === parentId;
      });
    };
    return {
      getSteps,
      lenSubSteps,
      getStepExtraDetails,
    };
  },
});
</script>
