<template>
    <p class="text-right">
        <button
            @click="showAll"
            class="mr-2 text-base font-semibold text-white transition duration-200 ease-in-out cursor-pointer last:mr-0 lg:mr-4"
            :class="{'text-white' : choiceIsSelected('all')}"
        >{{ trans.get('__JSON__.All resources') }}</button>

        <button
            @click="showOnlyLocalLanguage"
            class="mr-2 text-base font-semibold text-white transition duration-200 ease-in-out cursor-pointer last:mr-0 lg:mr-4"
            :class="{'text-white' : choiceIsSelected('local')}"
        >{{ trans.get('__JSON__.:locale resources', {'locale': language}) }}</button>

        <span v-if="language !== 'English'">
            <button
                @click="showEnglishAndLocalLanguage"
                class="mr-2 text-base font-semibold text-white transition duration-200 ease-in-out cursor-pointer last:mr-0 lg:mr-4"
                :class="{'text-white' : choiceIsSelected('local-and-english')}"
            >{{ trans.get('__JSON__.English and :locale resources', {'locale': language}) }}</button>
        </span>
    </p>
</template>

<script>
export default {
    props: {
        language: {
            type: String,
            required: true
        },
        initialChoice: {
            type: String,
            required: true
        },
    },

    data() {
        return {
            choice: this.initialChoice,
        }
    },

    methods: {
        choose(value) {
            this.choice = value;
            axios.patch(route('user.preferences.update', {'locale': 'en'}), {
                'resource-language': value,
            })
            .then(function () {
                window.location.reload(false);
            })
            .catch(function (error) {
                alert('Error!');
            });
        },
        showAll() {
            this.choose('all');
        },
        showOnlyLocalLanguage() {
            this.choose('local');
        },
        showEnglishAndLocalLanguage() {
            this.choose('local-and-english');
        },
        choiceIsSelected(value) {
            return this.choice === value;
        },
    },
}
</script>
