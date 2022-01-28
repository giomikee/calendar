<script>
	import { Styles, Container, Col, Row } from 'sveltestrap';
	import Calendar from './components/Calendar.svelte';

	let appointments = {28: 'Test appointment', 31: 'Test appointment'};
</script>

<Styles />

<main>
	<Container>
		<Row>
			<h1>Calendar</h1>
			

			<Col xs=12>
				<Calendar bind:appointments />
			</Col>

		</Row>
	</Container>
</main>