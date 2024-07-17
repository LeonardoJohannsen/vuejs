<script>
import CardComments from './CardComments.vue';
export default {
    name: 'CardPost',
    props: {
        post: { type: Object, required: true },
        user: { type: Object, required: true },
        comments: { type: Array, required: true },
    },
    components: {
        CardComments,
    },
    data() {
        return {
            showComments: false,
            closeComments: false
        };
    },
    created() {

    },
    methods: {

        toOpenComments() {
            this.showComments = true;
            this.closeComments = true;
        },
        toCloseComments() {
            this.showComments = false;
            this.closeComments = false;
        },

    }
}
</script>
<template>
    <div class="cardPost" :id="post.id">
        <div class="cardBody">
            <h3>{{ post.title }}</h3>
            <hr>
            <p>{{ post.body }}</p>
        </diV>
        <div class="cardBottom">
            <div class="bottomLeft" @click="toOpenComments">
                <svg width="40px" height="40px" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <g id="SVGRepo_bgCarrier" stroke-width="0"></g>
                    <g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round"></g>
                    <g id="SVGRepo_iconCarrier">
                        <path
                            d="M10 9H17M10 13H17M7 9H7.01M7 13H7.01M21 20L17.6757 18.3378C17.4237 18.2118 17.2977 18.1488 17.1656 18.1044C17.0484 18.065 16.9277 18.0365 16.8052 18.0193C16.6672 18 16.5263 18 16.2446 18H6.2C5.07989 18 4.51984 18 4.09202 17.782C3.71569 17.5903 3.40973 17.2843 3.21799 16.908C3 16.4802 3 15.9201 3 14.8V7.2C3 6.07989 3 5.51984 3.21799 5.09202C3.40973 4.71569 3.71569 4.40973 4.09202 4.21799C4.51984 4 5.0799 4 6.2 4H17.8C18.9201 4 19.4802 4 19.908 4.21799C20.2843 4.40973 20.5903 4.71569 20.782 5.09202C21 5.51984 21 6.0799 21 7.2V20Z"
                            stroke="#1e1e37" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"></path>
                    </g>
                </svg>
                <p><b>Carregar comentarios</b></p>
            </div>
            <div class="bottomCenter">
                <div v-if="closeComments" @click="toCloseComments">
                    <svg width="40px" height="40px" viewBox="0 0 1024 1024" xmlns="http://www.w3.org/2000/svg"
                        fill="#000000">
                        <g id="SVGRepo_bgCarrier" stroke-width="0"></g>
                        <g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round"></g>
                        <g id="SVGRepo_iconCarrier">
                            <path fill="#1e1e37"
                                d="M195.2 195.2a64 64 0 0 1 90.496 0L512 421.504 738.304 195.2a64 64 0 0 1 90.496 90.496L602.496 512 828.8 738.304a64 64 0 0 1-90.496 90.496L512 602.496 285.696 828.8a64 64 0 0 1-90.496-90.496L421.504 512 195.2 285.696a64 64 0 0 1 0-90.496z">
                            </path>
                        </g>
                    </svg>
                </div>
            </div>
            <div class="bottomRight">
                <svg width="40px" height="40px" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <g id="SVGRepo_bgCarrier" stroke-width="0"></g>
                    <g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round"></g>
                    <g id="SVGRepo_iconCarrier">
                        <path
                            d="M16 7C16 9.20914 14.2091 11 12 11C9.79086 11 8 9.20914 8 7C8 4.79086 9.79086 3 12 3C14.2091 3 16 4.79086 16 7Z"
                            stroke="#1e1e32" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"></path>
                        <path d="M12 14C8.13401 14 5 17.134 5 21H19C19 17.134 15.866 14 12 14Z" stroke="#1e1e32"
                            stroke-width="2" stroke-linecap="round" stroke-linejoin="round"></path>
                    </g>
                </svg>
                <p><b>{{ user.name }}</b></p>
            </div>
        </div>
        <div v-if="showComments" class="cardComments">
            <p v-for="comment in this.comments" :key="comment.id">
                <CardComments :comment="comment" />
            </p>
        </div>
    </div>

</template>
<style></style>