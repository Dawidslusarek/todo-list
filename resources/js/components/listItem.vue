<template>
    <div class="item">
        <input
            type="checkbox"
            @change="updateCheck()"
            v-model="item.completed"
        />
        <input
            type="text"
            v-model="item.name"
            :readonly="isReadOnly"
            :class="[item.completed ? 'completed' : '', 'itemText']"
            v-on:keyup.enter="updateCheck()"
        />
        <button @click="removeItem()" class="trashcan">
            <font-awesome-icon icon="trash" />
        </button>
    </div>
</template>

<script>
export default {
    props: ["item"],
    methods: {
        updateCheck() {
            axios
                .put("/api/item/" + this.item.id, {
                    item: this.item
                })
                .then(response => {
                    if (response.status == 200) {
                        this.$emit("itemchanged");
                    }
                })
                .catch(error => {
                    console.log(error);
                });
        },
        removeItem() {
            axios.delete("/api/item/" + this.item.id).then(response => {
                console.log("Usunięto pomyślnie!");
                this.$emit("itemchanged");
            });
        }
    },
    computed: {
        isReadOnly() {
            if (this.item.completed == 1) {
                return true;
            }
        }
    }
};
</script>

<style lang="scss" scoped>
.completed {
    text-decoration: line-through;
    color: #999;
}
.itemText {
    width: 100%;
    font-size: 18px;
    margin-left: 20px;
    font-weight: bold;
}
.item {
    display: flex;
    justify-content: center;
    align-items: center;
}
.trashcan {
    background: #e6e6e6;
    border: none;
    color: #ff0000;
    outline: none;
    cursor: pointer;
    font-size: 16px;
}
input {
    cursor: pointer;
    border: none;
    background: none;
}
input:focus {
    outline: none;
}
</style>
