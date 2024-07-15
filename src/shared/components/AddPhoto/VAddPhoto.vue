<template>
	<div
		class="photo-upload bg-lightGray mb-[8px] flex justify-center items-center"
		:style="{ height: `${heightMain}px` }"
	>
		<div
			v-if="uploadedImage"
			class="flex gap-[8px] items-center justify-center"
		>
			<img
				:src="uploadedImage"
				alt="Uploaded Photo"
				class="object-cover rounded"
				:style="{ width: `${widthImage}px`, height: `${heightImage}px` }"
			>
			<button
				class="p-2 cursor-pointer"
				@click.stop.prevent="removeImage"
			>
				<IconClose icon-color="#9F9FA0" />
			</button>
		</div>
		<label
			v-else
			class="photo-upload-label flex justify-center items-center"
		>
			<input
				type="file"
				class="photo-upload-input"
				@change="handleFileUpload"
			>
			<div class="photo-upload-content flex items-center gap-2">
				<IconPhoto />
				<span class="text-xs">{{ title }}</span>
			</div>
		</label>
	</div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { IconClose, IconPhoto } from '@/shared/components/Icon'

defineProps<{
	title: string;
	widthImage: number;
	heightImage: number;
	heightMain: number;
}>()

const uploadedImage = ref<string | null>(null)

const handleFileUpload = (event: Event) => {
    const target = event.target as HTMLInputElement
    const files = target.files
    if (files && files[0]) {
        const reader = new FileReader()
        reader.onload = (e: ProgressEvent<FileReader>) => {
            uploadedImage.value = e.target?.result as string
        }
        reader.readAsDataURL(files[0])
    }
}

const removeImage = () => {
    uploadedImage.value = null
}
</script>

<style scoped>
.photo-upload {
	@apply bg-lightGray rounded-[16px] text-center cursor-pointer;
}

.photo-upload-label {
	@apply flex flex-col items-center;
}

.photo-upload-input {
	@apply hidden;
}

</style>