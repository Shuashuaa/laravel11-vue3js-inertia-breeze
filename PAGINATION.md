# Pagination

### Contollers/ProductContoller.php
```
public function index()
{
    $products = Product::paginate(3);

    return Inertia::render('Frontend/Products/Index', [
        'products' => $products
    ]);
}
```

### Index.vue
> Create a component Pagination.vue and manipulate the links from products.data
```
import Pagination from '@/Components/Pagination.vue';

v-for="(item, index) in products.data" :key="index"

...
</table>

<Pagination class="mt-4" :links="products.links"/>
```

### js/Components/Pagination.vue
```
<script setup>
import { Link } from '@inertiajs/vue3';

defineProps({
    links: Array
})
</script>

<template>
    <div v-if="links.length > 0">
        <div class="flex flex-wrap -mb-1">
            <template v-for="(link, linkIndex) in links" :key="linkIndex">
                <div
                    v-if="link.url === null"
                    v-html="link.label"
                    class="mr-1 mb-1 px-4 py-3 text-sm leading-4 text-gray-400 border rounded"
                ></div>

                <Link
                    v-else
                    class="mr-1 mb-1 px-4 py-3 text-sm leading-4 border rounded hover:bg-gray-200 hover:text-gray-700
                    focus:border-indigo-500 inline-block focus:text-indigo-500"
                    
                    :class="{ 'bg-blue-700 text-white': link.active }"
                    :href="link.url"
                >
                    <span v-html="link.label"></span>
                </Link>
            </template>
        </div>
    </div>

</template>
```