<template>
  <h1>Photo Gallery</h1>
  <button @click="fetchPhotoGallery">Fetch Photo Data</button>
  <p>
    Total Photos: {{ totalPhotos }} ({{ oddPhotos.length }} odd albums |
    {{ evenPhotos.length }} even albums {{ evenAlbumPercentage }} percentage)
  </p>
  <ul>
    <li v-for="photo in photos" :key="photo.id">
      <img :src="photo.thumbnailUrl" />
    </li>
  </ul>
</template>

<script setup>
const photos = ref([]);

const fetchPhotoGallery = async () => {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/photos");
    if (!response.ok) {
      throw new Error("Network response was not ok");
    }
    const data = await response.json();
    photos.value = data;
  } catch (error) {
    console.error("Error fetching data:", error);
  }
};
const totalPhotos = computed(() => photos.value.length);
const evenPhotos = computed(() => {
  return photos.value.filter((photo) => photo.id % 2 === 0);
});
const oddPhotos = computed(() => {
  return photos.value.filter((photo) => photo.id % 2 !== 0);
});
const evenAlbumPercentage = computed(() => {
  if (totalPhotos.value === 0) {
    return 0;
  }
  return (evenPhotos.value.length / totalPhotos.value) * 100;
});
</script>
