<template>
  <form @submit="handleSubmit" class="space-y-4 sm:space-y-6">
    <div class="space-y-2 sm:space-y-3">
      <Label class="flex items-center gap-2 text-xs sm:text-sm font-semibold text-gray-200">
        <Upload :size="14" class="sm:w-4 sm:h-4 text-violet-400" />
        Upload Image
      </Label>

      <div
        :class="[
          'relative border-2 border-dashed rounded-lg p-4 sm:p-6 transition-all duration-200 cursor-pointer',
          {
            'border-violet-400 bg-violet-400/10': isDragOver,
            'border-gray-600 hover:border-violet-400/50 hover:bg-gray-800/50': !isDragOver
          }
        ]"
        @dragover="handleDragOver"
        @dragleave="handleDragLeave"
        @drop="handleDrop"
        @click="handleClick"
      >
        <input
          ref="fileInputRef"
          type="file"
          accept="image/*"
          @change="handleInputChange"
          class="hidden"
        />

        <div v-if="imageFile && imagePreview" class="flex items-center gap-3 sm:gap-4">
          <div class="relative">
            <img
              :src="imagePreview || '/placeholder.svg'"
              alt="Preview"
              class="w-12 h-12 sm:w-16 sm:h-16 object-cover rounded-lg border border-gray-600"
            />
          </div>
          <div class="flex-1 min-w-0">
            <p class="text-xs sm:text-sm font-medium text-gray-200 truncate">{{ imageFile.name }}</p>
            <p class="text-xs text-gray-400">{{ (imageFile.size / 1024 / 1024).toFixed(2) }} MB</p>
          </div>
          <Button
            type="button"
            variant="ghost"
            size="sm"
            @click.stop="clearFile()"
            class="h-7 w-7 sm:h-8 sm:w-8 p-0 hover:bg-red-500/10 hover:text-red-400 text-gray-400"
          >
            <X :size="14" class="sm:w-4 sm:h-4" />
          </Button>
        </div>

        <div v-else class="text-center">
          <div class="mx-auto w-10 h-10 sm:w-12 sm:h-12 bg-violet-400/10 rounded-lg flex items-center justify-center mb-2 sm:mb-3">
            <ImageIcon :size="20" class="sm:w-6 sm:h-6 text-violet-400" />
          </div>
          <p class="text-xs sm:text-sm font-medium text-gray-200 mb-1">
            Click to upload or drag and drop
          </p>
          <p class="text-xs text-gray-400">PNG, JPG, GIF up to 10MB</p>
        </div>
      </div>
    </div>

    <Button
      type="submit"
      class="w-full h-10 sm:h-12 bg-violet-600 hover:bg-violet-700 text-white font-semibold transition-all duration-200 hover:scale-[1.02] text-sm sm:text-base disabled:opacity-50 disabled:cursor-not-allowed"
      :disabled="!imageFile"
    >
      Generate QR Code
    </Button>
  </form>
</template>

<script setup lang="ts">
// "use client"

import { ref } from "vue"
import { Button } from "@/components/ui/button"
import { Label } from "@/components/ui/label"
import { Upload, X, ImageIcon } from "lucide-vue-next"

interface ImageInputProps {
  onQRCodeGenerated: (status: boolean) => void
}

const props = defineProps<ImageInputProps>()

const imageFile = ref<File | null>(null)
const isDragOver = ref(false)
const imagePreview = ref<string | null>(null)
const fileInputRef = ref<HTMLInputElement | null>(null)

const handleSubmit = (e: Event) => {
  e.preventDefault()
  if (imageFile.value) {
    // Generate QR code logic here
    console.log("[v0] Generating QR code for image:", imageFile.value.name)
    props.onQRCodeGenerated(true)
  }
}

const handleFileChange = (file: File) => {
  if (file && file.type.startsWith("image/")) {
    imageFile.value = file

    // Create preview
    const reader = new FileReader()
    reader.onload = (e) => {
      imagePreview.value = e.target?.result as string
    }
    reader.readAsDataURL(file)
  }
}

const handleInputChange = (e: Event) => {
  const target = e.target as HTMLInputElement
  const file = target.files?.[0]
  if (file) handleFileChange(file)
}

const handleDragOver = (e: DragEvent) => {
  e.preventDefault()
  isDragOver.value = true
}

const handleDragLeave = (e: DragEvent) => {
  e.preventDefault()
  isDragOver.value = false
}

const handleDrop = (e: DragEvent) => {
  e.preventDefault()
  isDragOver.value = false
  const file = e.dataTransfer?.files[0]
  if (file) handleFileChange(file)
}

const clearFile = () => {
  imageFile.value = null
  imagePreview.value = null
  if (fileInputRef.value) {
    fileInputRef.value.value = ""
  }
}

const handleClick = () => {
  fileInputRef.value?.click()
}
</script>