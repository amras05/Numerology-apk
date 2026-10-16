<template>
  <q-layout view="lHh Lpr lFf">
    <!-- Header -->
    <q-header elevated class="app-header border-b-glass">
      <q-toolbar class="q-px-md" style="min-height: 56px;">
        <q-avatar size="32px" class="q-mr-sm">
          <q-icon name="auto_awesome" color="primary" size="24px" />
        </q-avatar>
        <q-toolbar-title class="text-weight-bold text-subtitle1 tracking-wide font-outfit text-header-title">
          Numerology Calculator
        </q-toolbar-title>
        
        <div class="row items-center q-gutter-sm">
          <!-- Light/Dark Mode Toggle Button -->
          <q-btn
            flat
            round
            dense
            :icon="$q.dark.isActive ? 'light_mode' : 'dark_mode'"
            :color="$q.dark.isActive ? 'amber-4' : 'indigo-3'"
            @click="toggleDark"
          >
            <q-tooltip>Toggle {{ $q.dark.isActive ? 'Light' : 'Dark' }} Theme</q-tooltip>
          </q-btn>
          <q-badge color="primary" text-color="white" label="Offline SPA" class="text-weight-bold gt-xs" />
        </div>
      </q-toolbar>
    </q-header>

    <!-- Main Container -->
    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script setup>
import { useQuasar } from 'quasar'

const $q = useQuasar()

// Set dark mode active by default
$q.dark.set(true)

function toggleDark() {
  $q.dark.toggle()
}
</script>

<style lang="scss">
// Class styled globally to support both light and dark modes
.app-header {
  background: rgba(15, 23, 42, 0.75) !important;
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  color: #fff;
  transition: background-color 0.3s ease;
}

body.body--light {
  .app-header {
    background: rgba(255, 255, 255, 0.8) !important;
    color: #1e293b;
    border-bottom: 1px solid rgba(30, 41, 59, 0.08) !important;
  }
  .text-header-title {
    color: #1e293b !important;
  }
}

body.body--dark {
  .app-header {
    background: rgba(15, 23, 42, 0.75) !important;
    color: #ffffff;
    border-bottom: 1px solid rgba(255, 255, 255, 0.08) !important;
  }
  .text-header-title {
    color: #ffffff !important;
  }
}

.border-b-glass {
  border-bottom: 1px solid rgba(255, 255, 255, 0.08);
}
.tracking-wide {
  letter-spacing: 0.05em;
}
.font-outfit {
  font-family: 'Outfit', sans-serif;
}
</style>
