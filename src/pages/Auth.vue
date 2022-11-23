<template>
  <q-page class="flex flex-center">
    <div class="q-gutter-y-md" style="max-width: 900px; width: 100%">

      <q-card>
        <q-tabs
          v-model="tab"
          class="bg-grey-3 text-grey-7"
          active-color="primary"
          indicator-color="purple"
          align="justify"
        >
          <q-tab name="signIn" label="Sign In" />
          <q-tab name="signUp" label="Sign Up" />
        </q-tabs>

        <q-tab-panels v-model="tab" animated class="bg-white">
          <q-tab-panel name="signIn">
            <q-form
              @submit="onSubmit"
              @reset="onReset"
              class="q-gutter-md"
            >
              <q-input
                filled
                v-model="name"
                label="Email *"
                hint="enter you email"
                lazy-rules
                :rules="[ val => val && val.length > 0 || 'Please enter your email']"
              />

              <q-input
                filled
                type="password"
                v-model="password"
                label="password *"
                lazy-rules
                :rules="[
          val => val !== null && val !== '' || 'Please Enter your password'
        ]"
              />

<!--
              <q-toggle v-model="accept" label="I accept the license and terms" />
-->

              <div>
                <q-btn label="Login" type="submit" color="primary"/>
<!--
                <q-btn label="Reset" type="reset" color="primary" flat class="q-ml-sm" />
-->
              </div>
            </q-form>

          </q-tab-panel>

          <q-tab-panel name="signUp">
            <div class="text-h6">Sign Up</div>
            Lorem ipsum dolor sit amet consectetur adipisicing elit.
          </q-tab-panel>

        </q-tab-panels>
      </q-card>
    </div>


  </q-page>
</template>

<script>
import {ref} from "vue";
import {useQuasar} from "quasar";

export default {
  name: "Auth",
  setup () {
    const $q = useQuasar()

    const name = ref(null)
    const password = ref(null)
    const accept = ref(false)
    return {
      tab: ref('signIn'),
      name,
      password,
      accept,

      onSubmit () {
        if (accept.value !== true) {
          $q.notify({
            color: 'red-5',
            textColor: 'white',
            icon: 'warning',
            message: 'You need to accept the license and terms first'
          })
        }
        else {
          $q.notify({
            color: 'green-4',
            textColor: 'white',
            icon: 'cloud_done',
            message: 'Submitted'
          })
        }
      },

      onReset () {
        name.value = null
        password.value = null
        accept.value = false
      }

    }
  }
}
</script>

<style scoped>

</style>
