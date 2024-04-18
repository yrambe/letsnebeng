<template>
    <div class="pt-16">
        <h1 class="text-3xl font-semibold mb-4">Where are we going?</h1>
            <form action="#" @submit.prevent="">
                <div class="overflow-hidden shadow sm:rounded-md max-w-sm mx-auto text-left" >
                    <div class="bg-white px-4 py-5 sm:p-6">
                    <div>
                        <GMapAutocomplete
                        placeholder="My destination"
                        class="mt-1 block w-full px-3 py-2 rounded-md border border-gray-300 shadow-sm"
                        @place_changed="handleLocationChanged"
                        />
                    </div>
                    </div>
                    <div class="bg-gray-50 px-4 py-3 text-right sm:px-6">
                    <button
                        type="submit"
                        @submit.prevent=""
                        class="inline-flex justify-center rounded-md border border-transparent bg-black py-2 px-4 text-sm font-medium text-white shadow-sm hover:bg-gray-600 focus:outline-none"
                    >
                        Continue
                    </button>
                    </div>
                </div>
            </form>
    </div>
</template>

<script setup>
const handleLocationChanged = (e) => {
    console.log('handle location changed', e);
}

</script>