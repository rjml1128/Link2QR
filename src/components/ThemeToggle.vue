<template>
  <Button
    variant="ghost"
    size="sm"
    @click="setTheme(theme === 'light' ? 'dark' : 'light')"
    class="h-9 w-9 rounded-lg border-2 border-border/50 hover:border-primary/50 transition-all duration-200 hover:scale-105"
  >
    <Sun class="h-4 w-4 rotate-0 scale-100 transition-all dark:-rotate-90 dark:scale-0" />
    <Moon class="absolute h-4 w-4 rotate-90 scale-0 transition-all dark:rotate-0 dark:scale-100" />
    <span class="sr-only">Toggle theme</span>
  </Button>
</template>

<script setup lang="ts">
// "use client"

import { ref, watch } from "vue"
import { Moon, Sun } from "lucide-vue-next"
import { Button } from "@/components/ui/button"

const theme = ref(localStorage.getItem('theme') || 'light')

const setTheme = (newTheme: string) => {
  theme.value = newTheme
  localStorage.setItem('theme', newTheme)
  document.documentElement.classList.toggle('dark', newTheme === 'dark')
}

watch(theme, (newTheme) => {
  document.documentElement.classList.toggle('dark', newTheme === 'dark')
}, { immediate: true })
</script>