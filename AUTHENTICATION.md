# Authentication

### routes/web.php
> use the integrated middleware() function
```
Route::middleware('auth')->group(function(){
    Route::resource('products', ProductController::class);
});
```

- Register once to use the login (/register)
- Try to login (/products) auto redirect to (/login)

### js/Components/Navbar.vue
```
<li class="nav-item">
    <Link class="text-dark" :href="route('register')">Register</Link>
</li>
<li class="nav-item">
    <Link class="text-dark" :href="route('login')">Login</Link>
</li>
```

### js/Layouts/AuthenticatedLayout.vue
```
import Dropdown from '@/Components/Dropdown.vue';
import DropdownLink from '@/Components/DropdownLink.vue';

<Dropdown align="right" width="48">
    <template #trigger>
        <span class="inline-flex rounded-md">
            <button
                type="button"
                class="inline-flex items-center rounded-md border border-transparent bg-white px-3 py-2 text-sm font-medium leading-4 text-gray-500 transition duration-150 ease-in-out hover:text-gray-700 focus:outline-none"
            >
                {{ $page.props.auth.user.name }}

                <svg
                    class="-me-0.5 ms-2 h-4 w-4"
                    xmlns="http://www.w3.org/2000/svg"
                    viewBox="0 0 20 20"
                    fill="currentColor"
                >
                    <path
                        fill-rule="evenodd"
                        d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z"
                        clip-rule="evenodd"
                    />
                </svg>
            </button>
        </span>
    </template>

    <template #content>
        <DropdownLink
            :href="route('profile.edit')"
        >
            Profile
        </DropdownLink>
        <DropdownLink
            :href="route('logout')"
            method="post"
            as="button"
        >
            Log Out
        </DropdownLink>
    </template>
</Dropdown>
```

> Pass the copied AuthenticatedLayout components to `Navbar.vue`

### js/Components/Nabvar.vue
```
import Dropdown from '@/Components/Dropdown.vue';
import DropdownLink from '@/Components/DropdownLink.vue';

<li class="nav-item">
    <Link class="text-dark" :href="route('register')">Register</Link>
</li>
<li class="nav-item">
    <Link class="text-dark" :href="route('login')">Login</Link>
</li>

<Dropdown align="right" width="48">
    <template #trigger>
        <span class="inline-flex rounded-md">
            <button
                type="button"
                class="inline-flex items-center rounded-md border border-transparent bg-white px-3 py-2 text-sm font-medium leading-4 text-gray-500 transition duration-150 ease-in-out hover:text-gray-700 focus:outline-none"
            >
                {{ $page.props.auth.user.name }}

                <svg
                    class="-me-0.5 ms-2 h-4 w-4"
                    xmlns="http://www.w3.org/2000/svg"
                    viewBox="0 0 20 20"
                    fill="currentColor"
                >
                    <path
                        fill-rule="evenodd"
                        d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z"
                        clip-rule="evenodd"
                    />
                </svg>
            </button>
        </span>
    </template>

    <template #content>
        <DropdownLink
            :href="route('profile.edit')"
        >
            Profile
        </DropdownLink>
        <DropdownLink
            :href="route('logout')"
            method="post"
            as="button"
        >
            Log Out
        </DropdownLink>
    </template>
</Dropdown>
```

## Http/Middleware/HandleInertiaRequests.php
### Create a `checkAuth` for conditional block rendering
```
public function share(Request $request): array
{
    return [
        ...parent::share($request),
        'auth' => [
            'user' => $request->user(),
        ],
        'checkAuth' => $request->user() == true? true : false,
        'flash' => [
            'message' => fn () => $request->session()->get('message'),
        ],
    ];
}
```

## js/Components/Nabvar.vue
### Use `v-if` statements to hide and show login, register and dropdown menu
```
<li v-if="$page.props.checkAuth == false" class="nav-item">
    <Link class="text-dark" :href="route('register')">Register</Link>
</li>
<li v-if="$page.props.checkAuth == false" class="nav-item">
    <Link class="text-dark" :href="route('login')">Login</Link>
</li>

<Dropdown v-if="$page.props.checkAuth == true" align="right" width="48">
    <template #trigger>
        <span class="inline-flex rounded-md">
            <button
                type="button"
                class="inline-flex items-center rounded-md border border-transparent bg-white px-3 py-2 text-sm font-medium leading-4 text-gray-500 transition duration-150 ease-in-out hover:text-gray-700 focus:outline-none"
            >
                {{ $page.props.auth.user.name }}

                <svg
                    class="-me-0.5 ms-2 h-4 w-4"
                    xmlns="http://www.w3.org/2000/svg"
                    viewBox="0 0 20 20"
                    fill="currentColor"
                >
                    <path
                        fill-rule="evenodd"
                        d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z"
                        clip-rule="evenodd"
                    />
                </svg>
            </button>
        </span>
    </template>

    <template #content>
        <DropdownLink
            :href="route('profile.edit')"
        >
            Profile
        </DropdownLink>
        <DropdownLink
            :href="route('logout')"
            method="post"
            as="button"
        >
            Log Out
        </DropdownLink>
    </template>
</Dropdown>
```