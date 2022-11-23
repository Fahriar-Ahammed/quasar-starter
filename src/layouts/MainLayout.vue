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
          Quasar Starter
        </q-toolbar-title>
        <div v-if="$q.platform.is.desktop">
          <q-btn flat color="white" label="Home" to="/" />
          <q-btn flat color="white" label="Page1" to="page1" />
          <q-btn flat color="white" label="Page2" to="page2" />
        </div>
        <q-btn  class="glossy" rounded color="teal" label="Sign In"  to="login"/>

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
        <template v-for="route in essentialLinks">
          <q-item
            clickable
            v-ripple
            :active="route.link === this.path.substring(1)"
            active-class="my-menu-link"
            :to="route.link"
          >
            <q-item-section avatar>
              <q-icon :name="route.icon" />
            </q-item-section>

            <q-item-section>{{route.title}}</q-item-section>
          </q-item>
        </template>
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import {computed, defineComponent, ref} from 'vue'
import EssentialLink from 'components/EssentialLink.vue'
import {useRoute} from 'vue-router'


const linksList = [
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

export default defineComponent({
  name: 'MainLayout',

  components: {
    EssentialLink
  },

  setup () {
    const leftDrawerOpen = ref(false)
    const route=useRoute();
    const path = computed(() =>route.path)
    return {
      essentialLinks: linksList,
      leftDrawerOpen,
      path,
      toggleLeftDrawer () {
        leftDrawerOpen.value = !leftDrawerOpen.value
      }
    }
  }
})
</script>
