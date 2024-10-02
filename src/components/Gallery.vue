<!-- src/components/Gallery.vue -->
<template>
    <div class="container mx-auto p-4">
        <h1 class="text-2xl font-bold mb-4">Gallery Store</h1>
        <p>Your gateway to understand Vite & Vue and MongoDB</p>
        <br>
        <form @submit.prevent="uploadImage">
            <div class="flex items-center justify-center w-full">
                <label for="dropzone-file"
                    class="flex flex-col items-center justify-center w-full h-64 border-2 border-gray-300 border-dashed rounded-lg cursor-pointer bg-gray-50 dark:hover:bg-gray-800 dark:bg-gray-700 hover:bg-gray-100 dark:border-gray-600 dark:hover:border-gray-500 dark:hover:bg-gray-600">
                    <div class="flex flex-col items-center justify-center pt-5 pb-6">
                        <svg class="w-8 h-8 mb-4 text-gray-500 dark:text-gray-400" aria-hidden="true"
                            xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 20 16">
                            <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                d="M13 13h3a3 3 0 0 0 0-6h-.025A5.56 5.56 0 0 0 16 6.5 5.5 5.5 0 0 0 5.207 5.021C5.137 5.017 5.071 5 5 5a4 4 0 0 0 0 8h2.167M10 15V6m0 0L8 8m2-2 2 2" />
                        </svg>
                        <p class="mb-2 text-sm text-gray-500 dark:text-gray-400">
                            <span class="font-semibold">Click to upload</span> or drag and drop
                        </p>
                        <p class="text-xs text-gray-500 dark:text-gray-400">SVG, PNG, JPG or GIF</p>
                    </div>
                    <!-- File input -->
                    <input id="dropzone-file" type="file" @change="onFileChange" class="hidden" required />
                </label>
            </div>

            <!-- Title input and Upload button -->
            <div class="mt-4">
                <input v-model="title" type="text" placeholder="Title" class="border p-2 rounded w-full mb-4"
                    required />
                <button type="submit"
                    class="btn bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600">Upload</button>
            </div>


        </form>

        <div class="grid grid-cols-3 gap-4 mt-4">
            <div v-for="image in images" :key="image._id" class="border rounded p-2 relative">
                <img :src="`http://localhost:5000/${image.imagePath}`" alt="" class="w-full h-64 object-cover" />
                <br>
                <h5 class="text">{{ image.title }}</h5>

                <!-- Delete button with an icon -->
                <button @click="deleteImage(image._id)" class="absolute top-2 right-2 text-gray-500 hover:text-red-500">
                    <!-- Trash icon (SVG) -->
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2"
                        stroke="currentColor" class="w-6 h-6">
                        <path stroke-linecap="round" stroke-linejoin="round"
                            d="M19 7l-1 12a2 2 0 01-2 2H8a2 2 0 01-2-2L5 7m5 0V4a1 1 0 011-1h2a1 1 0 011 1v3m4 0H6" />
                    </svg>
                </button>
            </div>
        </div>

    </div>
</template>

<script>
import axios from 'axios';

export default {
    data() {
        return {
            title: '',
            file: null,
            images: [],
        };
    },
    methods: {
        onFileChange(event) {
            this.file = event.target.files[0];
        },
        async uploadImage() {
            const formData = new FormData();
            formData.append('image', this.file);
            formData.append('title', this.title);
            await axios.post('http://localhost:5000/upload', formData);
            this.title = '';
            this.file = null;
            this.fetchImages();
        },
        async fetchImages() {
            const response = await axios.get('http://localhost:5000/images');
            this.images = response.data;
        },
        async deleteImage(id) {
            await axios.delete(`http://localhost:5000/images/${id}`);
            this.fetchImages();
        },
    },
    mounted() {
        this.fetchImages();
    },
};
</script>

<style>
.btn {
    @apply bg-blue-500 text-white py-2 px-4 rounded;
}
</style>
