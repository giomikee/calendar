<script>
import { createEventDispatcher } from 'svelte';

    import {Modal, ModalBody, ModalFooter, Button, Input} from 'sveltestrap';

    let isOpen;
    let date;
    let day;
    let description;
    let isEditingAppointment;

    const dispatch = createEventDispatcher();

    export const toggle = (month, inputDay, inputDescription) => {
        isOpen = !isOpen;

        date = `${month} ${inputDay}`;
        day = inputDay
        description = inputDescription;

        if (inputDescription) {
            isEditingAppointment = true;
        }
    }

    const dispatchSaveAppointment = () => {
        dispatch('saveAppointment', {day, description});
        toggle();
    }


</script>

<Modal 
    header='{isEditingAppointment ? 'Edit' : 'Schedule'} Appointment'
    {isOpen} {toggle}>
    <ModalBody>
        <p>
            Date: <b>{date}</b>
        </p>

        <Input 
            bind:value={description}
            label='New appointment'
            type='textarea'
            placeholder='Appointment description'/>
    </ModalBody>    

    <ModalFooter>
        <Button 
            disabled={!description}
            color='primary' 
            on:click={dispatchSaveAppointment}>
            Save
        </Button>
        
        <Button 
            color='primary' outline 
            on:click={toggle}>
            Cancel
        </Button>
    </ModalFooter>
</Modal>