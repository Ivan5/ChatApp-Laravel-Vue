<template>
    <div class="chat-app">
        <Conversation :contact="selectedContact" :messages="messages" @new="saveNewMessage"/>
        <ContactList :contacts="contacts" @selected="startConversationWith"/>
    </div>
</template>

<script>
import Conversation from './Conversation';
import ContactList from './ContactList';
    export default {
        props:{
            user:{
                type: Object,
                required:true
            }
        },
        data(){
            return{
                selectedContact:null,
                messages:[],
                contacts:[]
            }
        },
        mounted() {
            console.log(this.user);
            
            axios.get('/contacts')
                .then((response) => {
                    this.contacts = response.data
                });
        },
        components:{
            Conversation,
            ContactList
        },
        methods:{
            startConversationWith(contact){
                axios.get(`/conversation/${contact.id}`)
                    .then(response => {
                        this.messages = response.data;
                        this.selectedContact = contact;
                    })
            },
            saveNewMessage(text){
                this.messages.push(text);
            }
        }
    }
</script>
<style>
.chat-app{
    display: flex;
}
</style>
