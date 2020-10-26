<template>
    <div>
        <b-nav tabs>
            <b-nav-item disabled> <b>Fancy quiz App</b></b-nav-item>
            <b-nav-item disabled>Questions: {{idquestionS+1}}/10</b-nav-item>
        </b-nav>
    </div>
</template>

<script>
export default {
    props: {
        idquestionS: Number
    }
}
</script>