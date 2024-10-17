<template>
    <h1>Administración de Tareas</h1>

    <h2>Tareas atrasadas</h2>
    <!--Instructions-->
    <p>Seleccione un rango de fechas y presione 'Submit':</p>
    <form @submit.prevent="getDelayedTasks">
        <!--Start Date-->
        <label for="startDate">Fecha de inicio:</label>
        <input type="date" id="startDate" name="startDate" required v-model="dateRange.startDate">
        <!--End Date-->
        <label for="endDate">Fecha de fin:</label>
        <input type="date" id="endDate" name="endDate" required v-model="dateRange.endDate">
        <!--Submit-->
        <input type="submit" value="Submit">
    </form>

    <!--Total-->
    <p v-if="tasks.length">Total: {{ tasks.length }}</p>
    <p v-else>No hay tareas atrasadas.</p>

    <!--Task Dashboard-->
    <table v-if="tasks.length">
        <thead>
            <tr>
                <th>Empleado</th>
                <th>Proyecto</th>
                <th>Asunto</th>
                <th>Descripción</th>
                <th>Estado</th>
                <th>Fecha de inicio</th>
                <th>Fecha final</th>
                <th>Días estimados</th>
                <th>Días atrasados</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="task in tasks" :key="task.id">
                <td>{{ task.owner }}</td>
                <td>{{ task.project }}</td>
                <td>{{ task.issue }}</td>
                <td>{{ task.description }}</td>
                <td>{{ task.status }}</td>
                <td>{{ task.startDate }}</td>
                <td>{{ task.endDate }}</td>
                <td>{{ task.estimatedDays }}</td>
                <td>{{ task.daysDelayed }}</td>
            </tr>
        </tbody>
    </table>

</template>

<script>

export default {
    name: 'Home',

    data() {
        return {
            dateRange: {
                startDate: '',
                endDate: '',
            },
            tasks: []
        }
    },

    methods: {
        getDelayedTasks() {
            fetch('http://localhost:8081/api/tasks/delayed', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify(this.dateRange)
            })
            .then(response => response.json())
            .then(body => {
                this.tasks = body.data;
            })
            .catch(error => {
                console.error('Error fetching tasks:', error);
            });
        }
    }
}

</script>
