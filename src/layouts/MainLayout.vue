<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-btn
          v-if="$q.platform.is.mobile"
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer"
        />

        <q-toolbar-title>
          Quasar Starter {{ loggedIn }}
        </q-toolbar-title>
        <div v-if="$q.platform.is.desktop">
          <q-btn flat color="white" label="Home" to="/"/>
          <q-btn flat color="white" label="Page1" to="page1"/>
          <q-btn flat color="white" label="Page2" to="page2"/>
        </div>
        <q-btn v-if="loggedIn === null || loggedIn === ''"   class="glossy" rounded color="teal" label="Sign In" to="login"/>
        <q-btn v-else  v-on:click="logout" class="glossy" rounded color="teal" label="logout" />

      </q-toolbar>
    </q-header>

    <q-drawer
      v-if="$q.platform.is.mobile"
      v-model="leftDrawerOpen"
      show-if-above
      bordered
    >
      <q-list>
        <q-item-label
          header
        >
          Admin Section
        </q-item-label>
        <template v-for="route in menuList">
          <q-item
            clickable
            v-ripple
            :active="route.link === path.substring(1)"
            active-class="my-menu-link"
            :to="route.link"
          >
            <q-item-section avatar>
              <q-icon :name="route.icon"/>
            </q-item-section>

            <q-item-section>{{ route.title }}</q-item-section>
          </q-item>
        </template>
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view/>
    </q-page-container>
  </q-layout>
</template>

<script setup>
import {computed, ref, watch} from 'vue'
import {useRoute,useRouter} from 'vue-router'
import { api } from 'boot/axios'


const menuList = [
  {
    title: 'Home',
    icon: 'home',
    link: '/'
  },
  {
    title: 'Page1',
    icon: 'code',
    link: 'page1'
  },
  {
    title: 'Page2',
    icon: 'code',
    link: 'page2'
  },
]
const leftDrawerOpen = ref(false)
const route = useRoute();
const router = useRouter()
const path = computed(() => route.path)

let loggedIn = ref(localStorage.getItem("token"))
console.log("##  "+loggedIn.value)
watch(loggedIn,(val) => {
  write(val)
});

function write(val){
  console.log("write  "+val)
}
// console.log("role  "+test.value)



function toggleLeftDrawer() {
  leftDrawerOpen.value = !leftDrawerOpen.value
}

function logout() {
  const token = localStorage.getItem("token")
  api.post('api/auth/logout?token=' + token)
    .then(() => {
      loggedIn.value = ""
      localStorage.setItem("role", "")
      localStorage.setItem("token", "")
      router.push('/')
    })
}


</script>
