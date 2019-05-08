<template>
    <div class="contacts-list">
        <ul>
            <li v-for="contact in sortedContacts" :key="contact.id" @click="selectContact(contact)" :class="{ 'selected': contact == selected }">
                <div class="contact">
                    <p class="name">{{ contact.name }}</p>
                    <p class="email">{{ contact.email }}</p>
                </div>
                <span class="unread" v-if="contact.unread">{{ contact.unread }}</span>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    props: {
        contacts: {
            type: Array,
            default: []

        }
    },
    data() {
        return{
            selected: this.contacts.length ? this.contacts[0] : null
        }
    },
    methods: {
        selectContact(contact){
            this.selected = contact;
            this.$emit('selected',contact);
        }
    },
    computed: {
        sortedContacts() {
            return _.sortBy(this.contacts, [(contact) =>{
                if(contact == this.selected){
                    return Infinity; 
                }
                return contact.unread;
            }]).reverse();
        }
    }
}
</script>
 
<style lang="scss" scoped>
    .contacts-list{
        flex: 2;
        max-height: 600px;
        overflow: auto;
        border-left: 2px solid lightgray;
        
        ul {
            list-style-type:none;
            padding-left:0;

            li{
                display: flex;
                padding: 2px 10px;
                border-bottom:1px solid lightgray;
                height: 50px;
                position:relative;
                cursor: pointer;

                &.selected{
                    background-color: lightgray;
                }
       
                span.unread{
                    background-color:#82e0a8;
                    color: #ffffff;
                    position: absolute;
                    right: 11px;
                    top: 10px;
                    display: flex;
                    font-weight: 700;
                    min-width: 15px;
                    justify-content: center;
                    align-items: center;
                    line-height: 15px;
                    font-size: 10px;
                    padding: 2px 4px 0px;
                    border-radius:3px;
                }

                .contact{
                    flex: 3;
                    font-size: 10px;
                    overflow: hidden;
                    display: flex;
                    flex-direction: column;
                    justify-content: center;

                    p {
                        margin: 0;
                        &.name{
                           font-weight: bold; 
                        }
                    }
                }
            }
        }
    }
</style>

