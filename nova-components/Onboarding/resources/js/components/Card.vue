<template>
  <VOnboardingWrapper ref="wrapper" :steps="steps" />
  <div>
    <button id="foo">Welcome</button>
  </div>
</template>

<script>
import { defineComponent, ref } from 'vue'
import { VOnboardingWrapper, useVOnboarding } from 'v-onboarding'
import 'v-onboarding/dist/style.css'
export default defineComponent ({
  components: {
    VOnboardingWrapper
    },
  setup() {
    const wrapper = ref(null)
    const steps = ref(null)
    const { start, goToStep, finish } = useVOnboarding(wrapper)

    Nova.request().get('/api/steps')
    .then(response => {
        const stepsTexts = response.data || [];
        const stepsDefault = ['User Menu', 'Details User', 'New User'];

        steps.value = [
            { attachTo: { element: '[href="/resources/users"]' }, content: { title: stepsTexts[0] || stepsDefault[0]} },
            { attachTo: { element: '[href="/resources/users/1"]' }, content: { title: stepsTexts[1] || stepsDefault[1] } },
            { attachTo: { element: '[href="/resources/users/new"]' }, content: { title: stepsTexts[2] || stepsDefault[2] } },
        ]
    })
    .catch(error => {
        console.error(error);
    });

    return {
        wrapper,
        steps,
        start
    }
    },
  mounted() {
    this.start()
  },
})
</script>
