<script>
import { createEventDispatcher } from 'svelte';

    import {Button, Modal, ModalBody, ModalFooter} from 'sveltestrap';

    let isOpen;
    let day;
    let date;
    let description;

    const dispatch = createEventDispatcher();

    export const toggle = (month, inputDay, inputDescription) => {
        isOpen = !isOpen;

        date = `${month} ${inputDay}`;
        day = inputDay;
        description = inputDescription;
    }

    const dispatchDeleteAppointment = () => {
        dispatch('deleteAppointment', {day});
        toggle();
    }

    const dispatchEditAppointment = () => {
        dispatch('editAppointment', {day, description});
        toggle();
    }
</script>

<Modal {isOpen} {toggle} header='Duplicate Appointment'>
    <ModalBody>
        <p>
            There is already another appointment scheduled on this date:
        </p>

        <p>
            <b>{date}: </b>
            {description}
        </p>
    </ModalBody>
    <ModalFooter>
        <Button 
            color='primary' on:click={dispatchEditAppointment}>
            Edit Appointment
        </Button>
        
        <Button 
            color='danger' on:click={dispatchDeleteAppointment}>
            Delete Appointment
        </Button>
        
        <Button 
            color='primary' outline on:click={toggle}>
            Close
        </Button>
    </ModalFooter>
</Modal>