<template>
    <section class="content-wrapper" style="min-height: 960px;">
        <section class="content-header">
            <h1>Employees</h1>
        </section>

        <section class="content">
            <div class="row">
                <div class="w-100">
                    <form @submit.prevent="submitForm">
                        <div class="box">
                            <div class="box-header with-border">
                                <h3 class="box-title">Edit</h3>
                            </div>

                            <div class="box-body">
                                <back-buttton></back-buttton>
                            </div>

                            <bootstrap-alert />

                            <div class="box-body">
                                <div class="form-group">
                                    <label for="company">Company</label>
                                    <v-select
                                            name="company"
                                            label="name"
                                            @input="updateCompany"
                                            :value="item.company"
                                            :options="companiesAll"
                                            />
                                </div>
                                <div class="form-group">
                                    <label for="first_name">First name</label>
                                    <input
                                            type="text"
                                            class="form-control"
                                            name="first_name"
                                            placeholder="Enter First name"
                                            :value="item.first_name"
                                            @input="updateFirst_name"
                                            >
                                </div>
                                <div class="form-group">
                                    <label for="last_name">Last name</label>
                                    <input
                                            type="text"
                                            class="form-control"
                                            name="last_name"
                                            placeholder="Enter Last name"
                                            :value="item.last_name"
                                            @input="updateLast_name"
                                            >
                                </div>
                                <div class="form-group">
                                    <label for="email">Email</label>
                                    <input
                                            type="email"
                                            class="form-control"
                                            name="email"
                                            placeholder="Enter Email"
                                            :value="item.email"
                                            @input="updateEmail"
                                            >
                                </div>
                                <div class="form-group">
                                    <label for="phone">Phone</label>
                                    <input
                                            type="text"
                                            class="form-control"
                                            name="phone"
                                            placeholder="Enter Phone"
                                            :value="item.phone"
                                            @input="updatePhone"
                                            >
                                </div>
                            </div>

                            <div class="box-footer">
                                <vue-button-spinner
                                        class="btn btn-primary btn-sm"
                                        :isLoading="loading"
                                        :disabled="loading"
                                        >
                                    Save
                                </vue-button-spinner>
                            </div>
                        </div>
                    </form>
                </div>
            </div>
        </section>
    </section>
</template>


<script>
import { mapGetters, mapActions } from 'vuex'

export default {
    data() {
        return {
            // Code...
        }
    },
    computed: {
        ...mapGetters('EmployeesSingle', ['item', 'loading', 'companiesAll']),
    },
    created() {
        this.fetchData(this.$route.params.id)
    },
    destroyed() {
        this.resetState()
    },
    watch: {
        "$route.params.id": function() {
            this.resetState()
            this.fetchData(this.$route.params.id)
        }
    },
    methods: {
        ...mapActions('EmployeesSingle', ['fetchData', 'updateData', 'resetState', 'setCompany', 'setFirst_name', 'setLast_name', 'setEmail', 'setPhone']),
        updateCompany(value) {
            this.setCompany(value)
        },
        updateFirst_name(e) {
            this.setFirst_name(e.target.value)
        },
        updateLast_name(e) {
            this.setLast_name(e.target.value)
        },
        updateEmail(e) {
            this.setEmail(e.target.value)
        },
        updatePhone(e) {
            this.setPhone(e.target.value)
        },
        submitForm() {
            this.updateData()
                .then(() => {
                    this.$router.push({ name: 'employees.index' })
                    this.$eventHub.$emit('update-success')
                })
                .catch((error) => {
                    console.error(error)
                })
        }
    }
}
</script>


<style scoped>

</style>
