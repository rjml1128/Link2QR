# Link2QR - Professional QR Code Creation Suite

> Transform your content into scannable QR codes with ease. Create professional-grade QR codes for links, images, WiFi credentials, and documents using our modern, intuitive web application.

## ✨ Features

- **Multiple Content Types**: Generate QR codes from links, images, WiFi networks, and documents
- **Professional Design**: High-quality, professional-grade QR code output
- **Responsive Design**: Fully responsive web application that works on desktop and mobile
- **Modern UI**: Clean, intuitive interface built with Vue 3 and TailwindCSS
- **Quick Actions**: Copy, share, and download QR codes instantly
- **Fast Generation**: Built with Vite for lightning-fast performance

## 🚀 Quick Start

### Prerequisites

- Node.js (version 16+)
- npm or yarn

### Installation

1. Clone the repository:
```bash
git clone https://github.com/rjml1128/Link2QR.git
cd Link2QR
```

2. Install dependencies:
```bash
npm install
```

3. Start the development server:
```bash
npm run dev
```

4. Open your browser and navigate to `http://localhost:5173`

### Build for Production

```bash
npm run build
```

## 🛠️ Tech Stack

- **Frontend Framework**: Vue 3 with Composition API
- **Type System**: TypeScript
- **Build Tool**: Vite
- **Styling**: TailwindCSS with custom components
- **UI Library**: Reka UI components
- **Icons**: Lucide Vue Next
- **Utilities**: VueUse, Tailwind Merge, Class Variance Authority

## 📁 Project Structure

```
Link2QR/
├── public/
│   └── vite.svg
├── src/
│   ├── components/
│   │   ├── ui/           # Reusable UI components
│   │   └── [Component].vue  # Main feature components
│   ├── lib/
│   │   └── utils.ts      # Utility functions
│   ├── assets/           # Static assets
│   └── *.ts              # Main application files
├── index.html            # Main HTML file
├── package.json          # Dependencies and scripts
├── tsconfig.json         # TypeScript configuration
└── vite.config.ts        # Vite build configuration
```

## 🎯 Usage

1. **Choose Content Type**: Select from Link, Image, WiFi, or Document
2. **Enter Details**: Fill in the required information for your QR code
3. **Generate**: Click generate to create your QR code
4. **Actions**: Copy, share, or download your QR code

## 📦 Scripts

- `npm run dev` - Start development server
- `npm run build` - Build for production
- `npm run preview` - Preview production build

## 🤝 Contributing

We welcome contributions! Please feel free to submit a Pull Request.

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

## 🙏 Acknowledgments

- [Vue.js](https://vuejs.org/) - The Progressive JavaScript Framework
- [Vite](https://vitejs.dev/) - Next Generation Frontend Tooling
- [TailwindCSS](https://tailwindcss.com/) - Utility-First CSS Framework
- [Reka UI](https://reka-ui.com/) - Modern Vue UI Library
