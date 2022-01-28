<script>
    import {Table, Button} from 'sveltestrap';
    import DuplicateAppointmentWarning from './DuplicateAppointmentWarning.svelte';
    import ScheduleAppointment from './ScheduleAppointment.svelte';

    export let appointments = {};
    const date = new Date();
    const monthTranslator = new Intl.DateTimeFormat('en-US', {month: 'long'});
    const currentMonth = date.getMonth();
    const currentYear = date.getFullYear();
    const currentDate = date.getDate();
    const currentMonthTranslated = monthTranslator.format(currentMonth);
    const daysInMonth = new Date(currentYear, currentMonth, 0).getDate();
    let duplicateAppointmentWarning, scheduleAppointment;


    const getDay = (rowNumber, columnNumber) => rowNumber * 7 + columnNumber + 1;
    

    const handleSchedulingAppointment = (day, dayHasAppointment) => {
        if (dayHasAppointment) {
            return duplicateAppointmentWarning.toggle(currentMonthTranslated, day, appointments[day])
        }

        return scheduleAppointment.toggle(currentMonthTranslated, day);

    }

    const deleteAppointment = (event) => {
        delete appointments[event.detail.day];
        appointments = {...appointments}
    }

    const editAppointment = event => {
        const {day} = event.detail;

        scheduleAppointment.toggle(currentMonthTranslated, day, appointments[day]);
    }

    const saveAppointment = event => {
        const {day, description} = event.detail;

        appointments = {
            ...appointments,
            [day]: description
        };
    }
</script>

<Table bordered>
    <thead>
        <tr>
            <th colspan="7">{currentMonthTranslated}</th>
        </tr>
    </thead>

    <tbody>
        {#each Array(5) as _, row (row)}
            <tr>
                {#each Array(7) as _, column (column)}
                    {@const day = getDay(row, column)}
                    {@const dayHasAppointment = appointments.hasOwnProperty(day)}

                    <td>
                        {#if day <= daysInMonth}
                            <Button 
                                disabled={day < currentDate}
                                color={dayHasAppointment ? 'secondary' : 'primary'}
                                on:click={() => handleSchedulingAppointment(day, dayHasAppointment)}>
                                {day}
                            </Button>
                        {/if}
                    </td>
                {/each}
                
            </tr>
        {/each}
    </tbody>
</Table>

<DuplicateAppointmentWarning 
    bind:this={duplicateAppointmentWarning}
    on:editAppointment={editAppointment}
    on:deleteAppointment={deleteAppointment} />

<ScheduleAppointment 
    bind:this={scheduleAppointment}
    on:saveAppointment={saveAppointment} />