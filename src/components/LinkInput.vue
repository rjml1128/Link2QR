<template>
  <form @submit="handleSubmit" class="space-y-4 sm:space-y-6">
    <div class="space-y-2 sm:space-y-3">
      <Label for="url" class="flex items-center gap-2 text-xs sm:text-sm font-semibold text-gray-200">
        <ExternalLink :size="14" class="sm:w-4 sm:h-4 text-violet-400" />
        Website URL
      </Label>
      <div class="relative">
        <div class="absolute left-2 sm:left-3 top-1/2 transform -translate-y-1/2 z-10">
          <Globe :size="16" class="sm:w-[18px] sm:h-[18px] text-gray-400" />
        </div>
        <Input
          id="url"
          type="url"
          placeholder="https://example.com"
          v-model="url"
          class="w-full h-10 sm:h-12 lg:h-14 pl-9 sm:pl-12 pr-10 sm:pr-12 text-sm sm:text-base border-2 border-gray-700 focus:border-violet-500 transition-all duration-200 bg-gray-900/50 backdrop-blur-sm text-gray-100 placeholder:text-gray-500"
        />
        <div v-if="url" class="absolute right-2 sm:right-3 top-1/2 transform -translate-y-1/2">
          <div v-if="isValidUrl" class="w-5 h-5 sm:w-6 sm:h-6 rounded-full bg-violet-500/20 flex items-center justify-center">
            <Check :size="12" class="sm:w-[14px] sm:h-[14px] text-violet-400" />
          </div>
          <div v-else class="w-1.5 h-1.5 sm:w-2 sm:h-2 rounded-full bg-red-500/50 animate-pulse"></div>
        </div>
      </div>
      <p class="text-xs text-gray-400 flex items-center gap-1">
        <span class="w-1 h-1 rounded-full bg-violet-400"></span>
        Enter a valid URL starting with http:// or https://
      </p>
    </div>
    <Button
      type="submit"
      class="w-full h-10 sm:h-12 bg-violet-600 hover:bg-violet-700 text-white font-semibold transition-all duration-200 hover:scale-[1.02] disabled:opacity-50 disabled:cursor-not-allowed text-sm sm:text-base"
      :disabled="!url.trim() || !isValidUrl"
    >
      Generate QR Code
    </Button>
  </form>
</template>

<script setup lang="ts">
// "use client"

import { ref, computed } from "vue"
import { Button } from "@/components/ui/button"
import { Input } from "@/components/ui/input"
import { Label } from "@/components/ui/label"
import { ExternalLink, Globe, Check } from "lucide-vue-next"

interface LinkInputProps {
  onQRCodeGenerated: (status: boolean) => void
}

const props = defineProps<LinkInputProps>()

const url = ref("")

const validateUrl = (value: string) => {
  try {
    new URL(value)
    return true
  } catch {
    return false
  }
}

const isValidUrl = computed(() => url.value.trim() !== "" && validateUrl(url.value))

const handleSubmit = (e: Event) => {
  e.preventDefault()
  if (url.value.trim() && isValidUrl.value) {
    console.log("[v0] Generating QR code for URL:", url.value)
    props.onQRCodeGenerated(true)
  }
}
</script>