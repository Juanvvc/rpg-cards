<template>
    <v-sheet
          :color="color"
          height="100%"
          tile
          :style="style"
        >
        <div class="d-flex flex-column fill-height justify-space-around align-center">
            <div v-if="title" class="text-h2">
                {{ title }}
            </div>
            <div v-if="text" class="text-body-1">
                {{ text }}
            </div>
            <div v-if="footer" class="text-overline font-italic">
                {{ footer }}
            </div>
        </div>      
    </v-sheet>
</template>

<script>
export default {
    props: {
        color: {default: "#ffffff00"},
        title: {default: null},
        text: {default: null},
        footer: {default: null},
        style: {default: ""}
    }
}
</script>