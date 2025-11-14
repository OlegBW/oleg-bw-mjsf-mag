<template>
  <div class="document-uploader">
    <div
      class="upload-zone"
      :class="{ 'drag-over': isDragging }"
      @dragover.prevent="handleDragOver"
      @dragleave="handleDragLeave"
      @drop.prevent="handleDrop"
      @click="handleClickUpload"
    >
      <input
        ref="fileInput"
        type="file"
        multiple
        class="file-input"
        @change="handleFileSelect"
      />
      <div class="upload-icon">📁</div>
      <p class="upload-text">
        {{
          isDragging ? "Drop files here" : "Drag files here or click to select"
        }}
      </p>
      <p class="upload-hint">
        Maximum {{ maxFiles }} {{ maxFiles === 1 ? "file" : "files" }}
      </p>
    </div>

    <div class="documents-grid">
      <div
        v-for="(file, index) in modelValue"
        :key="`uploaded-${index}`"
        class="document-preview"
      >
        <div class="document-image">
          <img :src="getImagePreview(file)" alt="Preview" />
        </div>
        <div class="document-info">
          <p class="document-name" :title="file.name">{{ file.name }}</p>
          <p class="document-size">{{ formatFileSize(file.size) }}</p>
        </div>
        <button
          class="delete-btn"
          @click="removeUploadedFile(index)"
          type="button"
        >
          ✕
        </button>
      </div>

      <div
        v-for="(doc, index) in modelDocuments"
        :key="`doc-${doc.type}-${index}`"
        class="document-preview"
      >
        <div class="document-image">
          <img :src="doc.icon" :alt="doc.name" />
        </div>
        <div class="document-info">
          <p class="document-name" :title="doc.name">{{ doc.name }}</p>
          <p class="document-type">{{ doc.type.toUpperCase() }}</p>
        </div>
        <button class="delete-btn" @click="removeDocument(index)" type="button">
          ✕
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";

const props = defineProps({
  modelValue: {
    type: Array,
    default: () => [],
  },
  documents: {
    type: Array,
    default: () => [],
  },
  maxFiles: {
    type: Number,
    default: 10,
  },
});

const emit = defineEmits(["update:modelValue", "update:documents"]);

const fileInput = ref(null);
const isDragging = ref(false);

const canAddMore = computed(() => props.modelValue.length < props.maxFiles);

const modelDocuments = computed({
  get: () => props.documents,
  set: (value) => emit("update:documents", value),
});

function handleDragOver(event) {
  isDragging.value = true;
  event.preventDefault();
}

function handleDragLeave() {
  isDragging.value = false;
}

function handleDrop(event) {
  isDragging.value = false;
  if (!canAddMore.value) return;

  const files = Array.from(event.dataTransfer.files);
  addFiles(files);
}

function handleClickUpload() {
  if (!canAddMore.value) return;
  fileInput.value?.click();
}

function handleFileSelect(event) {
  const files = Array.from(event.target.files);
  addFiles(files);
  event.target.value = "";
}

function addFiles(files) {
  const remainingSlots = props.maxFiles - props.modelValue.length;
  const filesToAdd = files.slice(0, remainingSlots);

  if (filesToAdd.length === 0) return;

  const updatedFiles = [...props.modelValue, ...filesToAdd];
  emit("update:modelValue", updatedFiles);
}

function removeUploadedFile(index) {
  const updatedFiles = props.modelValue.filter((_, i) => i !== index);
  emit("update:modelValue", updatedFiles);
}

function removeDocument(index) {
  const updatedDocs = modelDocuments.value.filter((_, i) => i !== index);
  emit("update:documents", updatedDocs);
}

function getImagePreview(file) {
  if (file.type.startsWith("image/")) {
    return URL.createObjectURL(file);
  }
  return "data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNjQiIGhlaWdodD0iNjQiIHZpZXdCb3g9IjAgMCAyNCAyNCIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KPHBhdGggZD0iTTEzIDlIMTJWMTJIMTNWN1oiIGZpbGw9IiM2NjYiLz4KPHBhdGggZD0iTTEwIDEySDJMMTIgMkgxMFYxMloiIGZpbGw9IiM2NjYiLz4KPC9zdmc+";
}

function formatFileSize(bytes) {
  if (bytes === 0) return "0 Bytes";
  const k = 1024;
  const sizes = ["Bytes", "KB", "MB", "GB"];
  const i = Math.floor(Math.log(bytes) / Math.log(k));
  return Math.round((bytes / Math.pow(k, i)) * 100) / 100 + " " + sizes[i];
}
</script>

<style scoped>
.document-uploader {
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
  font-family: "Inter", sans-serif;
}

/* Upload zone */
.upload-zone {
  border: 2px dashed #a3a3ff;
  border-radius: 16px;
  padding: 50px 20px;
  text-align: center;
  cursor: pointer;
  background: linear-gradient(145deg, #f8f8ff, #eef0ff);
  transition: all 0.3s ease;
  position: relative;
}

.upload-zone:hover {
  border-color: #6366f1;
  background: linear-gradient(145deg, #eef0ff, #dde0ff);
}

.upload-zone.drag-over {
  border-color: #4f46e5;
  background: linear-gradient(145deg, #e0e0ff, #c7caff);
  transform: scale(1.03);
}

/* Hidden file input */
.file-input {
  display: none;
}

/* Upload icon */
.upload-icon {
  font-size: 60px;
  margin-bottom: 15px;
  color: #4f46e5;
  transition: transform 0.3s ease;
}

.upload-zone:hover .upload-icon {
  transform: scale(1.1);
}

/* Upload text */
.upload-text {
  font-size: 18px;
  color: #1f2937;
  font-weight: 500;
  margin: 10px 0;
}

.upload-hint {
  font-size: 14px;
  color: #4b5563;
  margin-top: 5px;
}

/* Documents grid */
.documents-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 20px;
  margin-top: 20px;
}

/* Document card */
.document-preview {
  position: relative;
  border-radius: 12px;
  padding: 15px;
  background: #ffffff;
  box-shadow: 0 4px 12px rgba(79, 70, 229, 0.08);
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.document-preview:hover {
  transform: translateY(-3px);
  box-shadow: 0 6px 16px rgba(79, 70, 229, 0.15);
}

/* Document image */
.document-image {
  width: 100%;
  height: 150px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 12px;
  background: #eef0ff;
  border-radius: 8px;
  overflow: hidden;
}

.document-image img {
  max-width: 100%;
  max-height: 100%;
  object-fit: contain;
}

/* Document info */
.document-info {
  margin-top: 8px;
}

.document-name {
  font-size: 14px;
  font-weight: 600;
  color: #1f2937;
  margin: 0 0 4px 0;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.document-size,
.document-type {
  font-size: 12px;
  color: #6b7280;
  margin: 0;
}

/* Delete button */
.delete-btn {
  position: absolute;
  top: 10px;
  right: 10px;
  width: 30px;
  height: 30px;
  border: none;
  border-radius: 50%;
  background: #ef4444;
  color: white;
  font-size: 16px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s ease;
}

.delete-btn:hover {
  background: #b91c1c;
  transform: scale(1.1);
}

.delete-btn:active {
  transform: scale(0.95);
}

/* Responsive tweaks */
@media (max-width: 768px) {
  .documents-grid {
    grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
    gap: 15px;
  }

  .upload-zone {
    padding: 35px 15px;
  }

  .upload-icon {
    font-size: 50px;
  }

  .upload-text {
    font-size: 16px;
  }
}
</style>
