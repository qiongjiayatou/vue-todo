<template>
    <div class="d-flex justify-content-center mb-5">
        <form @submit.prevent="onSubmit">
            <div class="form-group">
                <input v-model="title" type="text" class="form-control" placeholder="Task...">
            </div>
            <button type="submit" class="btn btn-success">Submit</button>
        </form>
    </div>
    
</template>

<script>
export default {
    name: 'add-item',
    data() {
        return {
            title: ''
        }
    },
    methods: {
        onSubmit() {
            // console.log('submitted');
            this.$emit('add:item', this.title);
            this.title = '';
        }
    }
}
</script>