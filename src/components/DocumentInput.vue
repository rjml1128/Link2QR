<template>
  <form @submit="handleSubmit" class="space-y-4 sm:space-y-6">
    <div class="space-y-2 sm:space-y-3">
      <Label
        for="file_input"
        class="flex items-center gap-2 text-xs sm:text-sm font-semibold text-violet-300"
      >
        <FileText :size="14" class="sm:w-4 sm:h-4 text-violet-400" />
        Upload Document
      </Label>
      <div
        :class="[
          'relative border-2 border-dashed rounded-lg transition-all duration-200',
          {
            'border-violet-400 bg-violet-400/10': isDragOver,
            'border-red-400 bg-red-400/10': errorMessage,
            'border-violet-400 bg-violet-400/5': selectedFile,
            'border-gray-600 hover:border-violet-400/60 hover:bg-violet-400/5': !isDragOver && !errorMessage && !selectedFile
          }
        ]"
        @dragover="handleDragOver"
        @dragleave="handleDragLeave"
        @drop="handleDrop"
      >
        <input
          id="file_input"
          type="file"
          accept=".pdf,.doc,.docx,.xls,.xlsx,.ppt,.pptx,.txt"
          @change="handleInputChange"
          class="absolute inset-0 w-full h-full opacity-0 cursor-pointer z-10"
        />

        <div v-if="selectedFile" class="p-4 sm:p-6">
          <div class="flex items-center gap-3 sm:gap-4">
            <div class="w-10 h-10 sm:w-12 sm:h-12 rounded-lg bg-violet-400/20 flex items-center justify-center">
              <File :size="20" class="sm:w-6 sm:h-6 text-violet-400" />
            </div>
            <div class="flex-1 min-w-0">
              <p class="text-xs sm:text-sm font-medium text-gray-100 truncate">{{ fileName }}</p>
              <p class="text-xs text-gray-400">{{ formatFileSize(selectedFile.size) }}</p>
            </div>
            <div class="flex items-center gap-1 sm:gap-2">
              <CheckCircle :size="16" class="sm:w-5 sm:h-5 text-violet-400" />
              <button
                type="button"
                @click="clearSelection"
                class="p-1 hover:bg-red-400/20 rounded-md text-red-400 transition-colors"
              >
                <X :size="14" class="sm:w-4 sm:h-4" />
              </button>
            </div>
          </div>
        </div>

        <div v-else class="p-6 sm:p-8 text-center">
          <div class="w-12 h-12 sm:w-16 sm:h-16 mx-auto mb-3 sm:mb-4 rounded-full bg-violet-400/20 flex items-center justify-center">
            <Upload :size="20" class="sm:w-6 sm:h-6 text-violet-400" />
          </div>
          <p class="text-xs sm:text-sm font-medium text-gray-100 mb-1">
            {{ isDragOver ? "Drop your document here" : "Choose a document or drag it here" }}
          </p>
          <p class="text-xs text-gray-400">PDF, Word, Excel, PowerPoint, TXT (Max 10MB)</p>
        </div>
      </div>

      <div v-if="errorMessage" class="flex items-center gap-2 text-xs sm:text-sm text-red-400">
        <div class="w-1 h-1 rounded-full bg-red-400"></div>
        {{ errorMessage }}
      </div>
    </div>

    <Button
      type="submit"
      class="w-full h-10 sm:h-12 bg-violet-600 hover:bg-violet-700 text-white font-semibold transition-all duration-200 hover:scale-[1.02] disabled:opacity-50 disabled:cursor-not-allowed text-sm sm:text-base border-0"
      :disabled="!selectedFile || !!errorMessage || isLoading"
    >
      {{ isLoading ? "Processing..." : "Generate QR Code" }}
    </Button>
  </form>
</template>

<script setup lang="ts">
// "use client"

import { ref } from "vue"
import { Button } from "@/components/ui/button"
import { Label } from "@/components/ui/label"
import { FileText, X, Upload, File, CheckCircle } from "lucide-vue-next"

interface DocumentInputProps {
  onQRCodeGenerated: (status: boolean) => void
}

const props = defineProps<DocumentInputProps>()

const selectedFile = ref<File | null>(null)
const fileName = ref("")
const errorMessage = ref("")
const isLoading = ref(false)
const isDragOver = ref(false)

const handleFileChange = (file: File | null) => {
  errorMessage.value = ""

  if (file) {
    const validTypes = [
      "application/pdf",
      "application/msword",
      "application/vnd.openxmlformats-officedocument.wordprocessingml.document",
      "application/vnd.ms-excel",
      "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet",
      "application/vnd.ms-powerpoint",
      "application/vnd.openxmlformats-officedocument.presentationml.presentation",
      "text/plain",
    ]

    const maxSizeMB = 10
    const maxSizeBytes = maxSizeMB * 1024 * 1024

    if (!validTypes.includes(file.type)) {
      errorMessage.value = "Please choose a valid document type."
      return
    }

    if (file.size > maxSizeBytes) {
      errorMessage.value = "File exceeds maximum size of 10MB."
      return
    }

    selectedFile.value = file
    fileName.value = file.name
  } else {
    clearSelection()
  }
}

const handleInputChange = (e: Event) => {
  const target = e.target as HTMLInputElement
  const file = target.files?.[0] || null
  handleFileChange(file)
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
  const file = e.dataTransfer?.files[0] || null
  handleFileChange(file)
}

const clearSelection = () => {
  selectedFile.value = null
  fileName.value = ""
  errorMessage.value = ""
  const fileInput = document.getElementById("file_input") as HTMLInputElement
  if (fileInput) {
    fileInput.value = ""
  }
}

const formatFileSize = (bytes: number) => {
  if (bytes === 0) return "0 Bytes"
  const k = 1024
  const sizes = ["Bytes", "KB", "MB", "GB"]
  const i = Math.floor(Math.log(bytes) / Math.log(k))
  return parseFloat((bytes / Math.pow(k, i)).toFixed(2)) + " " + sizes[i]
}

const handleSubmit = async (e: Event) => {
  e.preventDefault()
  if (selectedFile.value && !errorMessage.value) {
    isLoading.value = true
    try {
      console.log("[v0] Generating QR code for document:", selectedFile.value.name)
      await new Promise((resolve) => setTimeout(resolve, 1000))
      props.onQRCodeGenerated(true)
    } catch (error) {
      console.error("Error processing document:", error)
      errorMessage.value = "Failed to process document."
    } finally {
      isLoading.value = false
    }
  }
}
</script>