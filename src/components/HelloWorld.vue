<template>
    <div>
        <div class="">
            <input type="text" v-model="author" class="card__input form-control mt-2" placeholder="Author">
            <input type="text" v-model="title" class="card__input form-control mt-2" placeholder="Title">
            <input type="text" v-model="content" class="card__input form-control mt-2" placeholder="Content">
            <button @click="postBlog()" class="save_button"><i class="fa fa-save"></i> Save</button>
        </div>
        <h1>author: {{ author }}</h1>
        <table class="table">
            <thead>
            <th>Author</th>
            <th>Title</th>
            <th>Content</th>
            </thead>
            <tbody>
            <tr v-for="blog in blogs" v-bind:key="blog._id">
                <td>{{blog.author}}</td>
                <td>{{blog.title}}</td>
                <td>{{blog.content}}</td>
                <td>
                    <button @click="getOne(blog)" class="secondary_button"><i class="fa fa-edit"></i>Edit</button>
                </td>
                <td>
                    <button @click="deleteOne(blog._id)" class="secondary_button"><i class="fa fa-trash"></i>Delete</button>
                </td>
            </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    name: 'HelloWorld',
    data(){
        return{
            blogs: null,
            url:'',
            author: '',
            title: '',
            content: '',
            // eslint-disable-next-line vue/no-reserved-keys
            _id: '',
            posts:{
                author:this.author,
                title:this.title,
                content:this.content
            }
        }
    },
    mounted() {
        this.getAll()
    },
    methods: {
        getAll(){
            axios.get(process.env.VUE_APP_API_BASE_URL)
                .then(response=>{
                    this.blogs=response.data;
                    this.author='';
                    this.title='';
                    this.content='';
                    this.url='';
                    this._id='';
                    //console.log(this._id)
                })
        },
        postBlog(){
            //if the url doesn't exist => create it
            if (this._id == ''){
                axios.post(process.env.VUE_APP_API_BASE_URL,
                    {author:this.author, title:this.title, content:this.content}
                )
                    .then(()=>{
                        this.getAll();
                    })
            }else { //if the address exists => edit it and SAVE by save-button
                axios.put(process.env.VUE_APP_API_BASE_URL,
                    {_id: this._id, author:this.author, title:this.title, content:this.content}
                    )
                    //getAll helps us make our app more dynamic (by reload data after changes)
                    .then(()=>{
                        this.getAll();
                    })
            }
        },
        deleteOne(_id){
            axios.delete(process.env.VUE_APP_API_BASE_URL + _id)
                //getAll helps us make our app more dynamic (by reload data after changes)
                .then(()=>{
                    this.getAll();
                })
        },
        getOne(blog){
            this._id = blog._id;
            this.author = blog.author;
            this.title = blog.title;
            this.content = blog.content;
        },
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
