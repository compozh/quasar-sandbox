<template lang="pug">
  q-page.flex.flex-center
    .q-pa-md(style="max-width: 400px")

      q-form(@submit="onSubmit"
             @reset="onReset").q-gutter-md

        q-input(filled v-model="name"
                label="Your name *"
                hint="Name and surname"
                lazy-rules
                :rules="[ val => val && val.length > 0 || 'Please type something']")

        q-input(filled type="number"
                v-model="age"
                label="Your age *"
                lazy-rules
                :rules=`[val => val !== null && val !== '' || 'Please type your age',
                         val => val > 0 && val < 100 || 'Please type a real age']`)

        q-toggle(v-model="accept" label="I accept the license and terms")

        div
          q-btn(label="Submit" type="submit" color="primary")
          q-btn(label="Reset" type="reset" color="primary" flat class="q-ml-sm")
</template>

<script>
export default {
  name: 'PageIndex'
}
</script>
