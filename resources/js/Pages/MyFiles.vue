<template>
    <AuthenticatedLayout>
        <nav class="flex items-center justify-between p-1 mb-3">
            <ol class="inline-flex items-center space-x-1 md:space-x-3">
                <li v-for="ans of ancestors.data" :key="ans.id" class="inline-flex items-center">
                    <Link v-if="!ans.parent_id" :href="route('myFiles')" class="inline-flex items-center text-sm font-medium text-gray-700 hover:text-blue-600">
                        <HomeIcon class="w-6 h-6 mr-2"/>
                         My Files
                    </Link>
                    <div v-else class="flex items-center">
                        <ChevronRightIcon class="w-4 h-4"/>
                        <Link :href="route('myFiles', {folder: ans.path})"
                              class="ml-1 text-sm font-medium text-gray-700 hover:text-blue-600 md:ml-2">
                            {{ ans.name }}
                        </Link>
                    </div>
                </li>
            </ol>
        </nav>
        <table class="min-w-full">
            <thead class="bg-gray-100 border-b">
                <tr>
                    <th class="text-sm font-medium text-gray-900 px-6 py-4 text-left">
                        Name
                    </th>
                    <th class="text-sm font-medium text-gray-900 px-6 py-4 text-left">
                        Owner
                    </th>
                    <th class="text-sm font-medium text-gray-900 px-6 py-4 text-left">
                        Last Modified
                    </th>
                    <th class="text-sm font-medium text-gray-900 px-6 py-4 text-left">
                        Size
                    </th>
                </tr>
            </thead>
            <tr v-for="file of files.data" :key="file.id" @dblclick="openFolder(file)" class="bg-white border-b transition duration-300 ease-in-out hover:bg-gray-100">
                <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-grat-900">
                    {{ file.name }}
                </td>
                <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-grat-900">
                    {{ file.owner }}
                </td>
                <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-grat-900">
                    {{ file.updated_at }}
                </td>
                <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-grat-900">
                    {{ file.size }}
                </td>
            </tr>
        </table>
        <div v-if="!files.data.length" class="py-8 text-center text-sm text-gray-400"> There is no data in the folder</div>
    </AuthenticatedLayout>
</template>

<script setup>
import AuthenticatedLayout from "@/Layouts/AuthenticatedLayout.vue";
import { router, Link } from "@inertiajs/vue3";
import { ChevronRightIcon, HomeIcon} from '@heroicons/vue/24/outline'


function openFolder(file) {
    if (!file.is_folder) {
        return;
    }

    router.visit(route('myFiles', {folder: file.path}))
}

const {files} = defineProps({
    files: Object,
    folder: Object,
    ancestors: Array
})

</script>