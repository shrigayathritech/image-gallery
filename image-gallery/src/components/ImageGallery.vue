<template>
  <div class="gallery">
    <!-- Image upload input -->
    <input type="file" @change="onImageUpload" accept="image/*" multiple />

    <!-- Gallery grid for image cards -->
    <div class="gallery-grid">
      <div
        v-for="(image, index) in images"
        :key="index"
        class="gallery-item"
        @click="openImage(index)"
      >
        <img :src="image.url" :alt="'Image ' + index" />
        <button class="delete-button" @click.stop="deleteImage(index)">✖</button>
      </div>
    </div>

    <!-- Enlarged image view with navigation and zoom controls -->
    <div v-if="enlargedImage" class="enlarged-image-overlay" @dblclick="closeImage">
      <img :src="enlargedImage.url" class="enlarged-image" :style="{ transform: `scale(${zoomLevel})` }" />
      <button class="nav-button prev" @click="prevImage">❮</button>
      <button class="nav-button next" @click="nextImage">❯</button>
      <div class="zoom-controls">
        <button @click="zoomIn">+</button>
        <button @click="zoomOut">-</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      images: [],
      enlargedImage: null,
      currentIndex: 0,
      zoomLevel: 1,
    };
  },
  methods: {
    // Handle image upload
    onImageUpload(event) {
      const files = event.target.files;
      if (files) {
        this.images.push(...Array.from(files).map(file => ({
          url: URL.createObjectURL(file),
        })));
      }
    },
    // Open image in enlarged view
    openImage(index) {
      this.enlargedImage = this.images[index];
      this.currentIndex = index;
    },
    // Close enlarged view
    closeImage() {
      this.enlargedImage = null;
      this.zoomLevel = 1; // Reset zoom level
    },
    // Show previous image
    prevImage() {
      this.currentIndex = (this.currentIndex - 1 + this.images.length) % this.images.length;
      this.enlargedImage = this.images[this.currentIndex];
    },
    // Show next image
    nextImage() {
      this.currentIndex = (this.currentIndex + 1) % this.images.length;
      this.enlargedImage = this.images[this.currentIndex];
    },
    // Zoom in the image
    zoomIn() {
      this.zoomLevel += 0.1;
    },
    // Zoom out the image
    zoomOut() {
      this.zoomLevel = Math.max(0.1, this.zoomLevel - 0.1);
    },
    // Delete image from gallery
    deleteImage(index) {
      this.images.splice(index, 1);
      if (this.enlargedImage && this.currentIndex === index) {
        this.closeImage();
      }
    },
  },
};
</script>

<style scoped>
.gallery {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
  background-color: #000;
}

.gallery-grid {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  justify-content: center;
}

.gallery-item {
  position: relative;
  cursor: pointer;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.gallery-item:hover {
  transform: scale(1.05);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.6);
}

.gallery-item img {
  width: 150px;
  height: 150px;
  object-fit: cover;
  border-radius: 8px;
}

.delete-button {
  position: absolute;
  top: 5px;
  right: 5px;
  background-color: rgba(0, 0, 0, 0.6);
  color: white;
  border: none;
  padding: 5px;
  cursor: pointer;
}

.enlarged-image-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.8);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.enlarged-image {
  max-width: 80%;
  max-height: 80%;
  transition: transform 0.3s ease;
}

.nav-button {
  position: absolute;
  top: 50%;
  color: white;
  background: transparent;
  border: none;
  font-size: 2rem;
  cursor: pointer;
}

.prev {
  left: 10%;
}

.next {
  right: 10%;
}

.zoom-controls {
  position: absolute;
  bottom: 20px;
  display: flex;
  gap: 10px;
}

.zoom-controls button {
  background: white;
  border: none;
  padding: 5px 10px;
  font-size: 1.5rem;
  cursor: pointer;
}
</style>
