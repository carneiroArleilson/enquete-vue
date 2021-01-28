<template>
    <div>
        <div class="row">
            <div class="col-md-2"></div>
            <div class="col-md-8">
                <div class="text-center">
                    <h1>Criar enquete</h1>
                </div>
                <div class="container-fluid">
                    <form @submit.prevent="store()" enctype="multipart/form-data">
                        <div class="form-row align-items-center">
                            <div class="col-sm-3 my-1">
                                <label class="sr-only" for="inlineFormInputName">Titulo</label>
                                <input type="text" class="form-control" id="inlineFormInputName"
                                       required v-model="poll.poll_description" placeholder="Informe a pergunta">
                            </div>
                            <div class="col-auto my-1">
                                <button type="submit" class="btn btn-primary">Registrar</button>
                            </div>
                        </div>
                    </form>
                    <hr>
                    <div class="container-fluid">
                        <table class="table">
                            <thead>
                            <tr>
                                <th scope="col">#</th>
                                <th scope="col">Perguntas</th>
                                <th scope="col">Ação</th>
                            </tr>
                            </thead>
                            <tbody>
                            <tr v-for="(poll, id) in polls" :key="poll.id">
                                <th scope="row">{{poll.id}}</th>
                                <td>{{poll.poll_description}}</td>
                                <td>
                                    <router-link :to="`poll/option/${poll.id}`">
                                        <button class="btn btn-info">Registrar respostas</button>
                                    </router-link>
                                    <button class="btn btn-danger" @click="destroy(poll.id)">Excluir</button>
                                </td>
                            </tr>
                            </tbody>
                        </table>
                    </div>
                </div>

            </div>
            <div class="col-md-2"></div>
        </div>
    </div>
</template>

<script>

import Poll from "../../model/Poll";
import PollList from "./PollList";
export default {
    components: {PollList},
    data() {
        return {
            poll: new Poll(),
            polls: [],
        }
    },
    methods: {
        store() {
            window.axios.post('http://localhost:8000/api/poll', this.poll)
                .then( () => {
                    this.poll = new Poll(), err => console.log(err)
                    this.index()
                })
        },
        index() {
            window.axios.get('http://localhost:8000/api/poll')
                .then(resp => {
                    this.polls = resp.data
                })
        },
        destroy(id) {
            window.axios.delete(`http://localhost:8000/api/poll/${id}`)
            .then(() => this.index())
        }
    },

    created() {
        this.index()
    }
}
</script>

<style>

</style>
