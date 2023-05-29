<template>
    <div :class="baseAlertClass">
        <slot name="text-alert"></slot>
    </div>
    
</template>

<script>
export default {
    name: "Alert-App",
    props: {
        variant: {
            type: String,
            default: ''
        }
    },
    computed: {
        baseAlertClass() {
            return [
                'alert',
                this.variant ? `alert-${this.variant}` :  ''
            ]
        }
    }
}
</script>

<style scoped>
.alert {
    color: #222;
    background-color: #fff;    
    border-radius: 7px;
    padding: 10px;
    max-width: 400px;
    margin: 10px auto;
    margin-bottom: 20px;
}

.alert-success {
    background-color: #42b983;
    color: #fff;
}

.alert-danger {
    background-color: #bd3038;
    color: #fff;
}

</style>