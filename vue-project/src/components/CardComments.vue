<script>
export default {
    name: 'CardComments',
    props: {
        comment: { type: Object, required: true },
    },
    created() {
        console.log('card comments');
        console.log(this.comment);
    }
}
</script>
<template>
    <div class="cardComment">
        <h4>{{ comment.name }}</h4>
        <p>{{ comment.body }}</p>
        <p class='author'>By {{ comment.email }}</p>
    </div>
</template>
<style></style>