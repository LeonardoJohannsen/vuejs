<script>
import LoadPosts from './LoadPosts.vue';
export default {
    name: 'MyHeader',
    components: {
        LoadPosts
    },
    data() {
        return {
            inputSearch: '',
            isLoged: false,
            storedUser: {
                username: '',
                nickname: '',
                email: '',
                password: '',
                confirmPassword: ''
            },
        };
    },
    created() {
        this.verfifyLogin();
    },
    methods: {
        showPosts() {
            this.showLoadPosts = true;
            this.showRegisterLogin = false;
        },
        showAuthScreen() {
            this.$router.push({ name: 'AuthScreen' });
        },
        searchPosts() {
           this.$refs.loadPostsComponent.filterPostsByTitle(this.inputSearch);
        },
        verfifyLogin(){
            this.storedUser = JSON.parse(sessionStorage.getItem('user'));
            if (this.storedUser != null) {
                this.isLoged = true;
            };
        },
    }
};

</script>
<template>
    <header>
        <div class="divTitle">
            <p id="textTitle">Lorem Ipsum <b>Blog</b></p>
        </div>
        <div class="divSearch">
            <div class="inputSearch">
                <input type="text" v-model="inputSearch" placeholder="Pesquise posts aqui." id="inputSearch">
                <button id="buttonSearch" @click="searchPosts">
                    <svg width="30px" height="30px" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <g id="SVGRepo_bgCarrier" stroke-width="0"></g>
                        <g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round"></g>
                        <g id="SVGRepo_iconCarrier">
                            <path
                                d="M15.7955 15.8111L21 21M18 10.5C18 14.6421 14.6421 18 10.5 18C6.35786 18 3 14.6421 3 10.5C3 6.35786 6.35786 3 10.5 3C14.6421 3 18 6.35786 18 10.5Z"
                                stroke="#f7f7fa" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"></path>
                        </g>
                    </svg>
                </button>
            </div>  
        </div>
        <div class="divIcons">
            <button @click="showAuthScreen">
                <svg width="40px" height="40px" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <g id="SVGRepo_bgCarrier" stroke-width="0"></g>
                    <g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round"></g>
                    <g id="SVGRepo_iconCarrier">
                        <path
                            d="M20 18L14 18M17 15V21M7.68213 14C8.63244 14.6318 9.77319 15 10.9999 15C11.7012 15 12.3744 14.8797 13 14.6586M10.5 21H5.6C5.03995 21 4.75992 21 4.54601 20.891C4.35785 20.7951 4.20487 20.6422 4.10899 20.454C4 20.2401 4 19.9601 4 19.4V17C4 15.3431 5.34315 14 7 14H7.5M15 7C15 9.20914 13.2091 11 11 11C8.79086 11 7 9.20914 7 7C7 4.79086 8.79086 3 11 3C13.2091 3 15 4.79086 15 7Z"
                            stroke="#f7f7fa" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"></path>
                    </g>
                </svg>
                <div id="loggedUser" v-if="isLoged">{{ storedUser.nickname }}</div>
            </button>
            
        </div>
    </header>
    <div class="divPosts">
        <LoadPosts ref="loadPostsComponent"/>   
    </div>
</template>
<style></style>