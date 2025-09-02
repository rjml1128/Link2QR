<template>
  <div class="min-h-screen lg:h-screen bg-background flex flex-col lg:overflow-hidden">
    <header class="w-full border-b border-border bg-card/30 backdrop-blur-xl flex-shrink-0 shadow-lg">
      <div class="container mx-auto flex items-center justify-between px-4 sm:px-6 py-3 sm:py-4">
        <div class="flex items-center gap-3 sm:gap-4">
          <div class="relative group">
            <div class="p-2 sm:p-3 bg-gradient-to-br from-primary to-primary/80 rounded-xl shadow-lg group-hover:shadow-xl group-hover:scale-105 transition-all duration-300 border border-primary/20">
              <QrCode class="w-5 h-5 sm:w-6 sm:h-6 text-primary-foreground" />
            </div>
          </div>
          <div>
            <h1 class="text-lg sm:text-2xl font-bold text-foreground font-sans tracking-tight">
              Link2QR
            </h1>
            <p class="text-xs sm:text-sm text-muted-foreground font-medium hidden sm:block">
              Professional QR Code Creation Suite
            </p>
          </div>
        </div>
        <div class="flex items-center gap-2 sm:gap-3">
          <Button v-if="selectedInput" variant="outline" size="sm" @click="handleBackToLanding" class="hover:bg-primary hover:text-primary-foreground transition-all duration-200 border-2 bg-transparent border-primary/30 text-primary hover:border-primary text-xs sm:text-sm px-2 sm:px-3 py-1 sm:py-2">
            <span class="hidden sm:inline">← Back to Home</span>
            <span class="sm:hidden">← Back</span>
          </Button>
        </div>
      </div>
    </header>

    <main class="flex-1 lg:overflow-auto">
      <div class="h-full flex items-center justify-center px-3 sm:px-4 py-4 sm:py-6">
        <div class="w-full max-w-7xl">
          <div v-if="!selectedInput" class="text-center space-y-4 sm:space-y-6 lg:space-y-8">
            <div class="space-y-3 sm:space-y-4 relative">
              <div class="relative">
                <div class="flex items-center justify-center gap-2 mb-2 sm:mb-3">
                  <Sparkles class="w-5 h-5 sm:w-6 sm:h-6 text-primary" />
                  <Badge variant="secondary" class="text-xs sm:text-sm font-bold uppercase tracking-wider px-3 py-1 bg-primary/20 text-primary border border-primary/30">
                    Try it now
                  </Badge>
                </div>
                <h2 class="text-2xl sm:text-3xl lg:text-4xl xl:text-5xl font-bold text-foreground font-sans leading-tight px-4 text-balance">
                  Create
                  <span class="bg-gradient-to-r from-primary to-accent bg-clip-text text-transparent">
                    Professional
                  </span>
                  QR Codes
                </h2>
                <p class="text-sm sm:text-base lg:text-lg text-muted-foreground max-w-3xl mx-auto leading-relaxed font-medium mt-2 sm:mt-3 px-4 text-pretty">
                  Transform your content into scannable QR codes with our advanced generator. Choose from links,
                  images, WiFi credentials, or documents with professional-grade quality.
                </p>
              </div>
            </div>

            <div class="space-y-3 sm:space-y-4">
              <div class="space-y-1 sm:space-y-2 px-4">
                <h3 class="text-xl sm:text-2xl font-bold text-foreground font-sans">
                  Choose Your Content Type
                </h3>
                <p class="text-sm sm:text-base text-muted-foreground font-medium">
                  Select the type of content you want to convert into a QR code
                </p>
              </div>
              <div class="grid grid-cols-2 lg:grid-cols-4 gap-3 sm:gap-4 max-w-5xl mx-auto px-4">
                <Card
                  v-for="item in inputTypes"
                  :key="item.type"
                  @click="handleInputTypeChange(item.type)"
                  class="cursor-pointer transition-all duration-200 hover:shadow-lg hover:scale-[1.02] border border-border hover:border-primary/50 group relative bg-card/80 backdrop-blur-sm hover:bg-card/90"
                >
                  <CardContent class="p-3 sm:p-4">
                    <div class="text-center space-y-2 sm:space-y-3">
                      <div class="flex items-center justify-center">
                        <div class="p-3 sm:p-4 rounded-xl bg-background/50 group-hover:bg-primary/10 shadow-sm group-hover:shadow-md transition-all duration-200 border border-border/30">
                          <component
                            :is="item.icon"
                            :class="`w-7 h-7 sm:w-9 sm:h-9 ${item.iconColor} group-hover:scale-105 transition-transform duration-200`"
                            stroke-width="1.5"
                          />
                        </div>
                      </div>

                      <div class="space-y-1 sm:space-y-2">
                        <h4 class="font-bold text-foreground text-base sm:text-xl font-sans">{{ item.label }}</h4>
                        <p class="text-xs sm:text-sm text-muted-foreground leading-relaxed font-medium">
                          {{ item.description }}
                        </p>
                      </div>
                    </div>
                  </CardContent>
                </Card>
              </div>
            </div>
          </div>

          <div v-else>
            <div class="mb-4 sm:mb-6">
              <div class="text-center mb-3 sm:mb-4 px-4">
                <h2 class="text-xl sm:text-2xl font-bold text-foreground font-sans mb-1 sm:mb-2">
                  Create {{ selectedInput }} QR Code
                </h2>
                <p class="text-sm sm:text-base text-muted-foreground font-medium">
                  Fill in the details below to generate your professional QR code
                </p>
              </div>
              <div class="flex items-center justify-center gap-2 sm:gap-3 flex-wrap px-4">
                <Button
                  v-for="item in inputTypes"
                  :key="item.type"
                  variant="outline"
                  @click="handleInputTypeChange(item.type)"
                  :class="cn(
                    'flex items-center gap-1 sm:gap-2 px-3 sm:px-4 py-2 sm:py-3 border-2 rounded-lg transition-all duration-300 font-medium text-xs sm:text-sm',
                    selectedInput === item.type
                      ? 'shadow-lg scale-105 hover:scale-105 bg-primary text-primary-foreground border-primary'
                      : 'hover:border-primary/50 hover:text-primary bg-background/50 hover:bg-primary/10 hover:scale-105 border-border',
                  )"
                >
                  <component
                    :is="item.icon"
                    :size="14"
                    stroke-width="1.5"
                    :class="selectedInput === item.type ? 'text-primary-foreground' : item.iconColor"
                  />
                  <span class="font-semibold">{{ item.label }}</span>
                </Button>
              </div>
            </div>

            <div class="flex flex-col xl:flex-row xl:items-start xl:justify-center gap-4 sm:gap-6 xl:gap-8 px-4">
              <div class="w-full xl:w-[480px] 2xl:w-[520px]">
                <Card class="shadow-xl border-2 border-border bg-card/60 backdrop-blur-sm">
                  <CardHeader class="pb-3">
                    <CardTitle class="text-base sm:text-lg font-sans text-card-foreground">
                      {{ selectedInput }} Details
                    </CardTitle>
                  </CardHeader>
                  <CardContent class="pt-0">
                    <LinkInput v-if="selectedInput === 'Link'" :onQRCodeGenerated="handleQRCodeGeneration" />
                    <ImageInput v-if="selectedInput === 'Image'" :onQRCodeGenerated="handleQRCodeGeneration" />
                    <WifiInput v-if="selectedInput === 'Wifi'" :onQRCodeGenerated="handleQRCodeGeneration" />
                    <DocumentInput v-if="selectedInput === 'Document'" :onQRCodeGenerated="handleQRCodeGeneration" />
                  </CardContent>
                </Card>
              </div>

              <div class="w-full xl:w-[360px] 2xl:w-[400px]">
                <Card class="shadow-xl border-2 border-border bg-card/60 backdrop-blur-sm">
                  <CardContent class="p-4 sm:p-6">
                    <div class="text-center space-y-4 sm:space-y-6">
                      <div class="space-y-1 sm:space-y-2">
                        <h4 :class="cn('text-base sm:text-lg font-bold font-sans', hasQRCode ? 'text-primary' : 'text-muted-foreground')">
                          {{ hasQRCode ? "Your QR Code is Ready!" : "Generate Your QR Code" }}
                        </h4>
                        <p v-if="!hasQRCode" class="text-xs sm:text-sm text-muted-foreground font-medium">
                          Fill out the form to generate your QR code
                        </p>
                      </div>

                      <div class="flex items-center justify-center border-2 border-dashed border-border rounded-xl shadow-inner aspect-square bg-background/30 p-4 sm:p-6 transition-all duration-300">
                        <div v-if="hasQRCode" id="qrcode" class="w-full h-full flex items-center justify-center" />
                        <QrCode v-else class="w-12 h-12 sm:w-16 sm:h-16 lg:w-20 lg:h-20 text-primary/40" stroke-width="0.5" />
                      </div>

                      <div v-if="hasQRCode" class="space-y-3 sm:space-y-4">
                        <p class="text-xs sm:text-sm font-bold text-card-foreground font-sans">
                          Quick Actions
                        </p>
                        <div class="flex flex-col sm:flex-row items-center justify-center gap-2 sm:gap-3">
                          <Button variant="outline" size="sm" @click="handleAction('copy')" class="w-full sm:w-auto flex items-center gap-1 sm:gap-2 hover:bg-primary hover:text-primary-foreground transition-all duration-300 font-medium text-xs border-2 rounded-lg px-3 py-2 border-primary/30 text-primary">
                            <Copy :size="14" stroke-width="1.5" />
                            Copy
                          </Button>
                          <Button variant="outline" size="sm" @click="handleAction('share')" class="w-full sm:w-auto flex items-center gap-1 sm:gap-2 hover:bg-accent hover:text-accent-foreground transition-all duration-300 font-medium text-xs border-2 rounded-lg px-3 py-2 border-accent/30 text-accent">
                            <Share :size="14" stroke-width="1.5" />
                            Share
                          </Button>
                          <Button variant="outline" size="sm" @click="handleAction('download')" class="w-full sm:w-auto flex items-center gap-1 sm:gap-2 hover:bg-primary hover:text-primary-foreground transition-all duration-300 font-medium text-xs border-2 rounded-lg px-3 py-2 border-primary/30 text-primary">
                            <Download :size="14" stroke-width="1.5" />
                            Download
                          </Button>
                        </div>
                      </div>
                    </div>
                  </CardContent>
                </Card>
              </div>
            </div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script setup lang="ts">
// "use client"

import { ref, reactive } from "vue"
import { Link, ImageIcon, Wifi, FileText, QrCode, Download, Share, Copy, Sparkles } from "lucide-vue-next"
import { Button } from "@/components/ui/button"
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card"
import { Badge } from "@/components/ui/badge"
import { cn } from "@/lib/utils"
import LinkInput from "@/components/LinkInput.vue"
import ImageInput from "@/components/ImageInput.vue"
import WifiInput from "@/components/WifiInput.vue"
import DocumentInput from "@/components/DocumentInput.vue"

type InputType = "Link" | "Image" | "Wifi" | "Document"

const selectedInput = ref<InputType | null>(null)
const hasQRCode = ref(false)

const inputTypes = [
  {
    type: "Link" as const,
    icon: Link,
    label: "Link",
    description: "Transform URLs into professional QR codes",
    iconColor: "text-primary",
  },
  {
    type: "Image" as const,
    icon: ImageIcon,
    label: "Image",
    description: "Convert images into scannable codes",
    iconColor: "text-accent",
  },
  {
    type: "Wifi" as const,
    icon: Wifi,
    label: "Wi-Fi",
    description: "Share network credentials instantly",
    iconColor: "text-primary",
  },
  {
    type: "Document" as const,
    icon: FileText,
    label: "Document",
    description: "Generate codes from your documents",
    iconColor: "text-accent",
  },
]

const handleInputTypeChange = (type: InputType) => {
  selectedInput.value = type
  hasQRCode.value = false
}

const handleBackToLanding = () => {
  selectedInput.value = null
  hasQRCode.value = false
}

const handleQRCodeGeneration = (status: boolean) => {
  hasQRCode.value = status
}

const handleAction = async (action: "copy" | "share" | "download") => {
  try {
    console.log(`[v0] Handling ${action} action`)
  } catch (error) {
    console.error(`Failed to handle ${action}:`, error)
  }
}
</script>