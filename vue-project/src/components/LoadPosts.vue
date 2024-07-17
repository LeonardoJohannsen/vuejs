<script>
import CardPost from './CardPost.vue';
export default {
    name: 'LoadPosts',
    components: {
        CardPost,

    },
    data() {
        return {
            posts: [],
            users: [],
            comments: [],
            mergedArray: [],
            perPage: 10,
            currentPage: 1,
            foundPosts: true,
        }
    },
    created() {
        Promise.all([this.getPosts(), this.getUsers(), this.getComments()])
            .then(() => {
                this.mergedArray = this.getMergedArray();
                console.log('criei');
            });
    },
    computed: {
        pageNumbers() {
            return Math.ceil(this.mergedArray.length / this.perPage);
        },
        paginatedPosts() {
            console.log('Paginei');
            const startIndex = (this.currentPage - 1) * this.perPage;
            const endIndex = startIndex + this.perPage;
            return this.mergedArray.slice(startIndex, endIndex);
        }
    },
    methods: {
        getUsers() {
            return fetch('https://jsonplaceholder.typicode.com/users')
                .then(response => response.json())
                .then(data => {
                    this.users = data;
                    console.log('users');
                });
        },

        getPosts() {
            return fetch('https://jsonplaceholder.typicode.com/posts')
                .then(response => response.json())
                .then(data => {
                    this.posts = data;
                    console.log('posts');
                });
        },
        getComments() {
            return fetch('https://jsonplaceholder.typicode.com/comments')
                .then(response => response.json())
                .then(data => {
                    this.comments = data;
                    console.log('Comments');
                });
        },
        getMergedArray() {
            return this.posts.map(post => {
                const user = this.users.find(user => user.id === post.userId);
                const filtredComments = this.comments.filter(comment => comment.postId === post.id);
                return { post, user, filtredComments };
            });
        },

        updatePosts(){
            this.mergedArray = this.getMergedArray();
            this.currentPage = 1;
        },

        filterPostsByTitle(title) {
            console.log(title);
            return this.getPosts().then(() => {
                this.posts = this.posts.filter(post => post.title.includes(title));
                console.log(this.posts.length);
                if(this.posts.length>0){
                    console.log(1); 
                    this.foundPosts = true;
                }else{
                    console.log(0);
                    this.foundPosts = false;
                };
                console.log("upadated");
                this.updatePosts();
            }).catch(error =>{
                console.log("deu erro" + error);
            })
        },
        firstPage() {
            console.log('oi');
            this.currentPage = 1;

        },
        prevPage() {
            console.log('oi');
            if (this.currentPage > 1) {
                this.currentPage--;

            }
        },
        nextPage() {
            console.log('oi');
            if (this.currentPage < this.pageNumbers) {
                this.currentPage++;

            }
        },
        lastPage() {
            console.log('oi');
            this.currentPage = this.pageNumbers;

        }
    },
}
</script>
<template>
    <div v-if="foundPosts">
        <p v-for="postWithUser in paginatedPosts" :key="postWithUser.post.id">
            <CardPost :post="postWithUser.post" :user="postWithUser.user" :comments="postWithUser.filtredComments" />
        </p>
    </div>
    <div v-else>
        <div class="cardPost">
            <h3>Não foram encontrados resultados relevantes à busca.</h3>
        </div>
    </div>
    <footer>
        <div class="pageControls">
            <div class="buttonControls" alt="Primeira pagina" @click="firstPage" :disabled="currentPage === 1">
                <svg width="30px" height="30px" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"
                    transform="rotate(0)">
                    <g id="SVGRepo_bgCarrier" stroke-width="0"></g>
                    <g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round"></g>
                    <g id="SVGRepo_iconCarrier">
                        <path
                            d="M3 5V19M12 7.329V16.671C12 17.7367 12 18.2695 11.7815 18.5432C11.5916 18.7812 11.3035 18.9197 10.9989 18.9194C10.6487 18.919 10.2327 18.5861 9.40049 17.9204L5.12347 14.4988C4.05578 13.6446 3.52194 13.2176 3.32866 12.7016C3.1592 12.2492 3.1592 11.7508 3.32866 11.2984C3.52194 10.7824 4.05578 10.3554 5.12348 9.50122L9.40049 6.07961C10.2327 5.41387 10.6487 5.081 10.9989 5.08063C11.3035 5.0803 11.5916 5.21876 11.7815 5.45677C12 5.73045 12 6.2633 12 7.329ZM21 7.329V16.671C21 17.7367 21 18.2695 20.7815 18.5432C20.5916 18.7812 20.3035 18.9197 19.9989 18.9194C19.6487 18.919 19.2327 18.5861 18.4005 17.9204L14.1235 14.4988C13.0558 13.6446 12.5219 13.2176 12.3287 12.7016C12.1592 12.2492 12.1592 11.7508 12.3287 11.2984C12.5219 10.7824 13.0558 10.3554 14.1235 9.50122L18.4005 6.07961C19.2327 5.41387 19.6487 5.081 19.9989 5.08063C20.3035 5.0803 20.5916 5.21876 20.7815 5.45677C21 5.73045 21 6.2633 21 7.329Z"
                            stroke="#1e1e37" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"></path>
                    </g>
                </svg>
            </div>
            <div class="buttonControls" alt="Pagina anterior" @click="prevPage" :disabled="currentPage === 1">
                <svg width="30px" height="30px" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"
                    transform="rotate(180)">
                    <g id="SVGRepo_bgCarrier" stroke-width="0"></g>
                    <g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round"></g>
                    <g id="SVGRepo_iconCarrier">
                        <path
                            d="M17 5V19M7 7.329V16.671C7 17.7367 7 18.2695 7.21846 18.5432C7.40845 18.7812 7.69654 18.9197 8.00108 18.9194C8.35125 18.919 8.76734 18.5861 9.59951 17.9204L13.8765 14.4988C14.9442 13.6446 15.4781 13.2176 15.6713 12.7016C15.8408 12.2492 15.8408 11.7508 15.6713 11.2984C15.4781 10.7824 14.9442 10.3554 13.8765 9.50122L9.59951 6.07961C8.76734 5.41387 8.35125 5.081 8.00108 5.08063C7.69654 5.0803 7.40845 5.21876 7.21846 5.45677C7 5.73045 7 6.2633 7 7.329Z"
                            stroke="#1e1e37" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"></path>
                    </g>
                </svg>
            </div>
            <div class="pageStatus">
                <p id="pageStatus"><b>{{ currentPage }} de {{ pageNumbers }}</b></p>
            </div>
            <div class="buttonControls" alt="Proxima pagina" @click="nextPage" :disabled="currentPage === pageNumbers">
                <svg width="30px" height="30px" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <g id="SVGRepo_bgCarrier" stroke-width="0"></g>
                    <g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round"></g>
                    <g id="SVGRepo_iconCarrier">
                        <path
                            d="M17 5V19M7 7.329V16.671C7 17.7367 7 18.2695 7.21846 18.5432C7.40845 18.7812 7.69654 18.9197 8.00108 18.9194C8.35125 18.919 8.76734 18.5861 9.59951 17.9204L13.8765 14.4988C14.9442 13.6446 15.4781 13.2176 15.6713 12.7016C15.8408 12.2492 15.8408 11.7508 15.6713 11.2984C15.4781 10.7824 14.9442 10.3554 13.8765 9.50122L9.59951 6.07961C8.76734 5.41387 8.35125 5.081 8.00108 5.08063C7.69654 5.0803 7.40845 5.21876 7.21846 5.45677C7 5.73045 7 6.2633 7 7.329Z"
                            stroke="#1e1e37" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"></path>
                    </g>
                </svg>
            </div>
            <div class="buttonControls" alt="Ultima pagina" @click="lastPage" :disabled="currentPage === pageNumbers">
                <svg width="30px" height="30px" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"
                    transform="rotate(180)">
                    <g id="SVGRepo_bgCarrier" stroke-width="0"></g>
                    <g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round"></g>
                    <g id="SVGRepo_iconCarrier">
                        <path
                            d="M3 5V19M12 7.329V16.671C12 17.7367 12 18.2695 11.7815 18.5432C11.5916 18.7812 11.3035 18.9197 10.9989 18.9194C10.6487 18.919 10.2327 18.5861 9.40049 17.9204L5.12347 14.4988C4.05578 13.6446 3.52194 13.2176 3.32866 12.7016C3.1592 12.2492 3.1592 11.7508 3.32866 11.2984C3.52194 10.7824 4.05578 10.3554 5.12348 9.50122L9.40049 6.07961C10.2327 5.41387 10.6487 5.081 10.9989 5.08063C11.3035 5.0803 11.5916 5.21876 11.7815 5.45677C12 5.73045 12 6.2633 12 7.329ZM21 7.329V16.671C21 17.7367 21 18.2695 20.7815 18.5432C20.5916 18.7812 20.3035 18.9197 19.9989 18.9194C19.6487 18.919 19.2327 18.5861 18.4005 17.9204L14.1235 14.4988C13.0558 13.6446 12.5219 13.2176 12.3287 12.7016C12.1592 12.2492 12.1592 11.7508 12.3287 11.2984C12.5219 10.7824 13.0558 10.3554 14.1235 9.50122L18.4005 6.07961C19.2327 5.41387 19.6487 5.081 19.9989 5.08063C20.3035 5.0803 20.5916 5.21876 20.7815 5.45677C21 5.73045 21 6.2633 21 7.329Z"
                            stroke="#1e1e37" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"></path>
                    </g>
                </svg>
            </div>
        </div>
    </footer>
</template>
<style></style>