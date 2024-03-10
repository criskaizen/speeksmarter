<script>
export default {
    name: "CategoriesIndex",
};
</script>
<script setup>
import AppLayout from "@/Layouts/AppLayout.vue";
import { Head, Link } from "@inertiajs/vue3";
import { router } from '@inertiajs/vue3'

defineProps({
    categories: {
        type:Object,
        required: true
    }
});
const deleteCategory = (id) => {
    if(confirm('Are you sure?')){
        router.delete(route('categories.destroy', id));
    }
}


</script>

<template>
    <AppLayout title="Categories">
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                Categories
            </h2>
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div v-if="$page.props.user.permissions.includes('create categories')" class="p-6 bg-white border-b border-gray-200">
                    <div class="flex justify-between">
                        <Link :href="route('categories.create')" class="text-white bg-indigo-500 hover:gb-indigo-700 py-2 px-4">Create Category</Link>
                    </div>
                </div>
                <div class="mt-4">
                    <ul role="list" class="divide-y divide-gray-100">
                        <li v-for="({id, name}, index) in categories.data" :key="index" class="flex justify-between gap-x-6 py-5">
                            <div class="flex min-w-0 gap-x-4">
                                <div class="min-w-0 flex-auto">
                                    <p class="text-md font-semibold leading-6 text-gray-900">
                                        {{ name }}
                                    </p>
                                </div>
                            </div>
                            <div class="hidden shrink-0 sm:flex sm:flex-col sm:items-end">
                                <p class="text-sm leading-6 text-gray-900">
                                    <Link class="py-1 px-2" :href="route('categories.edit', id)" v-if="$page.props.user.permissions.includes('update categories')">Edit</Link> | 
                                    <button class="py-1 px-2 text-red-500" @click="deleteCategory(id)" v-if="$page.props.user.permissions.includes('delete categories')">Delete</button>
                                </p>
                            </div>
                        </li>
                        
                    </ul>
                </div>
                <div class="flex justify-between mt-2">
                    <Link v-if="categories.current_page > 1" :href="categories.prev_page_url" class="py-2 px-4">Previous</Link>
                    
                    <Link v-if="categories.current_page < categories.last_page" :href="categories.next_page_url" class="py-2 px-4">Next</Link>
                </div>
            </div>
        </div>
    </AppLayout>
</template>
