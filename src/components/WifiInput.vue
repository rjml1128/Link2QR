<template>
  <form @submit="handleSubmit" class="space-y-4 sm:space-y-6">
    <div class="space-y-2 sm:space-y-3">
      <Label for="ssid" class="flex items-center gap-2 text-xs sm:text-sm font-semibold text-gray-200">
        <Wifi :size="14" class="sm:w-4 sm:h-4 text-violet-400" />
        Network Name (SSID)
      </Label>
      <Input
        id="ssid"
        placeholder="My WiFi Network"
        v-model="ssid"
        class="w-full h-10 sm:h-12 text-sm sm:text-base bg-gray-900 border-2 border-gray-700 text-gray-100 placeholder:text-gray-500 focus:border-violet-500 transition-colors"
      />
    </div>
    <div class="space-y-2 sm:space-y-3">
      <Label for="password" class="flex items-center gap-2 text-xs sm:text-sm font-semibold text-gray-200">
        <Lock :size="14" class="sm:w-4 sm:h-4 text-violet-400" />
        Password
      </Label>
      <div class="relative">
        <Input
          id="password"
          :type="showPassword ? 'text' : 'password'"
          placeholder="Enter WiFi password"
          v-model="password"
          class="w-full h-10 sm:h-12 text-sm sm:text-base bg-gray-900 border-2 border-gray-700 text-gray-100 placeholder:text-gray-500 focus:border-violet-500 transition-colors pr-10 sm:pr-12"
        />
        <Button
          type="button"
          variant="ghost"
          size="sm"
          @click="showPassword = !showPassword"
          class="absolute right-1 sm:right-2 top-1/2 -translate-y-1/2 h-7 w-7 sm:h-8 sm:w-8 p-0 hover:bg-gray-800 text-gray-400 hover:text-violet-400"
        >
          <EyeOff v-if="showPassword" :size="14" class="sm:w-4 sm:h-4" />
          <Eye v-else :size="14" class="sm:w-4 sm:h-4" />
        </Button>
      </div>
    </div>
    <div class="space-y-2 sm:space-y-3">
      <Label for="security" class="flex items-center gap-2 text-xs sm:text-sm font-semibold text-gray-200">
        <Lock :size="14" class="sm:w-4 sm:h-4 text-violet-400" />
        Security Type
      </Label>
      <div class="relative">
        <Select v-model="security">
          <SelectTrigger class="w-full h-12 sm:h-14 lg:h-16 text-sm sm:text-base bg-gray-900 border-2 border-gray-700 text-gray-100 focus:border-violet-500 transition-colors">
            <div class="flex items-center gap-2 sm:gap-3 py-1 sm:py-2">
              <div :class="['w-1.5 h-1.5 sm:w-2 sm:h-2 rounded-full', getSecurityDetails(security).color]"></div>
              <div class="font-medium text-xs sm:text-sm lg:text-base text-gray-100">
                {{ getSecurityDetails(security).label }}
                <span v-if="getSecurityDetails(security).description" class="text-gray-400 font-normal ml-1 sm:ml-2 hidden sm:inline">
                  {{ getSecurityDetails(security).description }}
                </span>
              </div>
            </div>
          </SelectTrigger>
          <SelectContent class="bg-gray-900 border-2 border-gray-700 shadow-lg">
            <SelectItem
              value="WPA"
              class="h-12 sm:h-14 lg:h-16 text-sm sm:text-base hover:bg-violet-500/20 focus:bg-violet-500/20 py-2 sm:py-3 text-gray-100"
            >
              <div class="flex items-center gap-2 sm:gap-3">
                <div class="w-1.5 h-1.5 sm:w-2 sm:h-2 rounded-full bg-violet-500"></div>
                <div>
                  <div class="font-medium text-xs sm:text-sm">WPA/WPA2</div>
                  <div class="text-xs text-gray-400 hidden sm:block">Most secure (recommended)</div>
                </div>
              </div>
            </SelectItem>
            <SelectItem
              value="WEP"
              class="h-12 sm:h-14 lg:h-16 text-sm sm:text-base hover:bg-violet-500/20 focus:bg-violet-500/20 py-2 sm:py-3 text-gray-100"
            >
              <div class="flex items-center gap-2 sm:gap-3">
                <div class="w-1.5 h-1.5 sm:w-2 sm:h-2 rounded-full bg-orange-500"></div>
                <div>
                  <div class="font-medium text-xs sm:text-sm">WEP</div>
                  <div class="text-xs text-gray-400 hidden sm:block">Legacy security</div>
                </div>
              </div>
            </SelectItem>
            <SelectItem
              value="nopass"
              class="h-12 sm:h-14 lg:h-16 text-sm sm:text-base hover:bg-violet-500/20 focus:bg-violet-500/20 py-2 sm:py-3 text-gray-100"
            >
              <div class="flex items-center gap-2 sm:gap-3">
                <div class="w-1.5 h-1.5 sm:w-2 sm:h-2 rounded-full bg-red-500"></div>
                <div>
                  <div class="font-medium text-xs sm:text-sm">No Password</div>
                  <div class="text-xs text-gray-400 hidden sm:block">Open network</div>
                </div>
              </div>
            </SelectItem>
          </SelectContent>
        </Select>
      </div>
    </div>
    <div class="space-y-2 sm:space-y-3">
      <Label for="hidden" class="flex items-center gap-2 text-xs sm:text-sm font-semibold text-gray-200">
        <AlertTriangle :size="14" class="sm:w-4 sm:h-4 text-violet-400" />
        Hidden Network
      </Label>
      <div class="flex items-center justify-between p-3 sm:p-4 border-2 border-gray-700 rounded-lg bg-gray-900 hover:bg-gray-800 transition-colors">
        <div class="space-y-0.5 sm:space-y-1">
          <p class="text-xs sm:text-sm font-medium text-gray-200">Network is hidden</p>
          <p class="text-xs text-gray-400">Enable if your network doesn't broadcast its name</p>
        </div>
        <Switch
          id="hidden"
          :checked="isHidden"
          @update:checked="isHidden = $event"
          class="data-[state=checked]:bg-violet-500 data-[state=unchecked]:bg-gray-600 data-[state=unchecked]:border-gray-500 border-2 shadow-lg scale-90 sm:scale-100"
        />
      </div>
    </div>
    <Button
      type="submit"
      class="w-full h-10 sm:h-12 bg-violet-600 hover:bg-violet-700 text-white font-semibold transition-all duration-200 hover:scale-[1.02] disabled:opacity-50 disabled:cursor-not-allowed text-sm sm:text-base"
      :disabled="!ssid.trim()"
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
import { Select, SelectContent, SelectItem, SelectTrigger, SelectValue } from "@/components/ui/select"
import { Switch } from "@/components/ui/switch"
import { Wifi, Lock, Eye, EyeOff, AlertTriangle } from "lucide-vue-next"

interface WifiInputProps {
  onQRCodeGenerated: (status: boolean) => void
}

const props = defineProps<WifiInputProps>()

const ssid = ref("")
const password = ref("")
const security = ref("WPA")
const showPassword = ref(false)
const isHidden = ref(false)

const getSecurityDetails = (value: string) => {
  switch (value) {
    case "WPA":
      return { label: "WPA/WPA2", description: "Most secure (recommended)", color: "bg-violet-500" }
    case "WEP":
      return { label: "WEP", description: "Legacy security", color: "bg-orange-500" }
    case "nopass":
      return { label: "No Password", description: "Open network", color: "bg-red-500" }
    default:
      return { label: "Select security type", description: "", color: "bg-gray-500" }
  }
}

const handleSubmit = (e: Event) => {
  e.preventDefault()
  if (ssid.value.trim()) {
    console.log("[v0] Generating QR code for WiFi:", { ssid: ssid.value, security: security.value, hidden: isHidden.value })
    props.onQRCodeGenerated(true)
  }
}
</script>