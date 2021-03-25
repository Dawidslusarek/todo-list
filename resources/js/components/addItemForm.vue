<template>
<div class="Additem">
<input type="text" v-model="item.name" v-on:keyup.enter="addItem()" />
<font-awesome-icon
icon="plus-square"
@click="addItem()"
:class="[item.name ? 'active' : 'inactive', 'plus']"
/>
</div>
</template>

<script>
export default {
    data: function() {
        return {
            item: {
                name: ""
            }
        }
    },
    methods: {
        addItem() {
            if(this.item.name == '') {
                return;
            }

            axios.post('api/item/store', {
                item: this.item
            })
            .then(response => {
                if(response.status == 201) {
                    this.$emit('itemchanged')
                    this.item.name = "";
                }
            })
            .catch( error => {
                console.log(error);
            })
        }
    }
}
</script>
<style scoped>
.Additem{
    display:flex;
    justify-content: center;
    align-items: center;
}
input{
    background: #f7f7f7;
    border:none;
    outline: none;
    padding: 5px;
    margin-right: 10px;
    width:100%;
}
.plus{
    font-size:25px;

}
.active{
    color: #00ce25;
    cursor: pointer;
}
.inactive{
    color: #a5a5a5;
    cursor:not-allowed;
}
</style>