<template>
    <div>
        <div class="card mb-3 mx-auto" style="max-width: 20rem;">
            <div class="card-header font-weight-bold" style="font-size:20px;">{{task.title}}</div>
            <div class="card-body text-primary">
                <p class="card-text"> Point: <span class="font-weight-bold"> {{task.point}} </span> </p>
                <p class="card-text">Assigned To: <span class="font-weight-bold"> {{task.assign}} </span> </p>
                <div class="container">
                    <button type="button" class="btn btn-primary" v-if="task.status != 'BackLog'" @click="back(task)"><i class="fas fa-arrow-circle-left"></i></button>
                    <button type="button" class="btn btn-primary" @click="deleteTask" >  <i class="fas fa-trash"></i> </button>
                    <button type="button" class="btn btn-primary" @click="next(task)" v-if="task.status != 'Done'" ><i class="fas fa-arrow-circle-right"></i></button>
                    <button type="button" class="btn btn-primary" data-toggle="modal" :data-target="'#'+task.id"> Detail </button>

                </div>
            </div>
        </div>


        <!-- Modal -->
        <div class="modal fade" :id="task.id" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog" role="document">
            <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title" id="exampleModalLabel">{{task.title}}</h5>
                <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                <span aria-hidden="true">&times;</span>
                </button>
            </div>
            <div class="modal-body">
                <p class="card-text"> Point: <span class="font-weight-bold"> {{task.point}} </span> </p>
                <p class="card-text"> Status: <span class="font-weight-bold"> {{task.status}} </span> </p>
                <p class="card-text">Assigned To: <span class="font-weight-bold"> {{task.assign}} </span> </p>
                <p class="card-text">Task Description: </p>
                <p class="card-text">{{task.description}}</p>
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
            </div>
            </div>
        </div>
        </div>


    </div>
</template>

<script>
    import db from '@/assets/config.js'

export default {
    name: 'CardTask',
    props: ['task'],
    data(){
        return {}
    },
    methods: {
        next(task){
            if(task.status === 'BackLog'){
                db.ref('/' + task.id).set({
                    status: 'ToDo',
                    title: task.title,
                    point: task.point,
                    assign: task.assign,
                    description: task.description,
                }, err => {
                    if (err) {
                        console.log(err);
                    } else {
                        console.log('berhasil');     
                    }
                })
            } else if(task.status === 'ToDo'){
                db.ref('/' + task.id).set({
                    status: 'Doing',
                    title: task.title,
                    point: task.point,
                    assign: task.assign,
                    description: task.description,
                }, err => {
                    if (err) {
                        console.log(err);
                    } else {
                        console.log('berhasil');     
                    }
                })
            } else if (task.status === 'Doing'){
                db.ref('/' + task.id).set({
                    status: 'Done',
                    title: task.title,
                    point: task.point,
                    assign: task.assign,
                    description: task.description,
                }, err => {
                    if (err) {
                        console.log(err);
                    } else {
                        console.log('berhasil');     
                    }
                })
            }
        },
        back(task){
            if(task.status === 'Done'){
                db.ref('/' + task.id).set({
                    status: 'Doing',
                    title: task.title,
                    point: task.point,
                    assign: task.assign,
                    description: task.description,
                }, err => {
                    if (err) {
                        console.log(err);
                    } else {
                        console.log('berhasil');     
                    }
                })
            } else if(task.status === 'Doing'){
                db.ref('/' + task.id).set({
                    status: 'ToDo',
                    title: task.title,
                    point: task.point,
                    assign: task.assign,
                    description: task.description,
                }, err => {
                    if (err) {
                        console.log(err);
                    } else {
                        console.log('berhasil');     
                    }
                })
            } else if(task.status === 'ToDo'){
                db.ref('/' + task.id).set({
                    status: 'BackLog',
                    title: task.title,
                    point: task.point,
                    assign: task.assign,
                    description: task.description,
                }, err => {
                    if (err) {
                        console.log(err);
                    } else {
                        console.log('berhasil');     
                    }
                })
            }
        },
        deleteTask(){
            db.ref(`/${this.task.id}`).remove()
        }
    }
}
</script>

<style>
    .btn{
        margin: 10px;
    }
</style>
