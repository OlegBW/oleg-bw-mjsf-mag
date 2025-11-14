<script setup lang="ts">
import { useBreakpoints } from '@/composables/useBreakpoints'

const {
  width,
  isMobileView,
  isTabletView,
  isDesktopView,
  greater,
  smaller,
  between,
} = useBreakpoints({
  mobile: 640,
  tablet: 960,
  desktop: 1280
})
</script>

<template>
  <div class="breakpoints-demo">
    <h1>🌐 Responsive Breakpoints Demo</h1>
    
    <!-- Window width -->
    <div class="card width-display">
      <p>📏 Current window width: <strong>{{ width }}px</strong></p>
    </div>

    <!-- Device indicator -->
    <div class="card device-indicator">
      <p v-if="isMobileView" class="device-mobile">📱 Mobile View</p>
      <p v-else-if="isTabletView" class="device-tablet">📟 Tablet View</p>
      <p v-else-if="isDesktopView" class="device-desktop">🖥 Desktop View</p>
      <p v-else class="device-other">📐 Intermediate Size</p>
    </div>

    <!-- Function checks -->
    <div class="card function-tests">
      <h2>🔍 Breakpoint Functions</h2>
      
      <div class="test-results">
        <p>
          <span>greater("tablet")</span> → 
          <span :class="greater('tablet') ? 'success' : 'error'">
            {{ greater('tablet') }}
          </span>
        </p>
        <p>
          <span>smaller("desktop")</span> → 
          <span :class="smaller('desktop') ? 'success' : 'error'">
            {{ smaller('desktop') }}
          </span>
        </p>
        <p>
          <span>between("mobile", "desktop")</span> → 
          <span :class="between('mobile', 'desktop') ? 'success' : 'error'">
            {{ between('mobile', 'desktop') }}
          </span>
        </p>
      </div>
    </div>

    <!-- Current breakpoints -->
    <div class="card info-box">
      <h3>🛠 Configured Breakpoints</h3>
      <p>Mobile: <strong>640px</strong></p>
      <p>Tablet: <strong>960px</strong></p>
      <p>Desktop: <strong>1280px</strong></p>
    </div>
  </div>
</template>

<style scoped>
/* Container */
.breakpoints-demo {
  max-width: 800px;
  margin: 2rem auto;
  padding: 2rem;
  font-family: 'Inter', sans-serif;
  color: #1f2937;
}

/* Headings */
h1 {
  font-size: 2rem;
  font-weight: 700;
  margin-bottom: 2rem;
  text-align: center;
  color: #1d4ed8;
}

h2 {
  font-size: 1.5rem;
  font-weight: 600;
  margin-bottom: 1rem;
  color: #2563eb;
}

h3 {
  font-size: 1.25rem;
  font-weight: 600;
  margin-bottom: 0.5rem;
  color: #1e40af;
}

/* Card style */
.card {
  background: #f9fafb;
  border: 1px solid #e5e7eb;
  border-radius: 12px;
  padding: 1.5rem;
  margin-bottom: 1.5rem;
  box-shadow: 0 2px 6px rgba(0,0,0,0.05);
}

/* Width display */
.width-display p {
  font-size: 1.125rem;
  font-weight: 600;
}

/* Device indicator */
.device-indicator p {
  font-size: 1.25rem;
  font-weight: 700;
  text-align: center;
  padding: 0.75rem 0;
  border-radius: 8px;
}

/* Device colors */
.device-mobile { color: #3b82f6; }
.device-tablet { color: #10b981; }
.device-desktop { color: #8b5cf6; }
.device-other { color: #f97316; }

/* Function tests */
.test-results p {
  display: flex;
  justify-content: space-between;
  font-size: 0.95rem;
  margin: 0.5rem 0;
}

.test-results span:first-child { font-weight: 600; }

/* Success / Error */
.success { color: #16a34a; font-weight: 600; }
.error { color: #dc2626; font-weight: 600; }

/* Info box */
.info-box p { font-size: 0.95rem; color: #1e293b; }
.info-box strong { color: #1d4ed8; }
</style>
