<script>
    import {onMount} from 'svelte';
    import {
        Button,
        Card,
        CardBody,
        CardHeader,
        CardSubtitle,
        CardTitle,
        Col,
        Container,
        FormGroup,
        Input,
        Label,
        Row,
			CardFooter
    } from 'sveltestrap';

    let voices = [];
    let rate = 1;
    let pitch = 1;
    let volume = 1;
    let text = 'Hello, World';
    let selectedVoice = null;

    const printVoice = voice => {
        if (!voice) return ''
        return `${voice.name} (${voice.lang})`;
    }
    onMount(() => {
        speechSynthesis.onvoiceschanged = () => {
            voices = speechSynthesis.getVoices();
            selectedVoice = voices[0];
        }
    });

    const play = () => {
    	const utterance = new SpeechSynthesisUtterance(text);
    	speechSynthesis.cancel();
    	utterance.rate = rate;
    	utterance.pitch = pitch;
    	utterance.voice = selectedVoice;
    	utterance.volume = volume;
    	speechSynthesis.speak(utterance);
    }
</script>

<main>
    <Container>
        <Card>
            <CardHeader>
                <CardTitle>Speech Synthesis</CardTitle>
                <CardSubtitle>Speak To Me</CardSubtitle>
            </CardHeader>
            <CardBody>
                <Row>
                    <Col>
                        <FormGroup>
                            <Label for="words">Say Something</Label>
                            <Input type="text" bind:value={text} id="words"/>
                        </FormGroup>
                    </Col>
                </Row>
                <Row>
                    <Col>
                        <FormGroup>
                            <Label for="voices">Voices</Label>
                            <Input type="select" bind:value={selectedVoice} id="words">
                                {#each voices as voice (voice.name)}
                                    <option value={voice}>{printVoice(voice)}</option>
                                {/each}
                            </Input>
                        </FormGroup>
                    </Col>
                </Row>
                <Row>
                    <Col>
                        <FormGroup>
                            <Label for="pitch">Pitch</Label>
                            <Input step="0.1" min="0.1" max="2" id="pitch" bind:value={pitch} type="range"/>
                        </FormGroup>
                    </Col>
                </Row>

                <Row>
                    <Col>
                        <FormGroup>
                            <Label for="rate">Rate</Label>
                            <Input step="0.1" min="0.1" max="2" id="rate" bind:value={rate} type="range"/>
                        </FormGroup>
                    </Col>
                </Row>

                <Row>
                    <Col>
                        <FormGroup>
                            <Label for="volume">Volume</Label>
                            <Input step="0.1" min="0.1" max="1" id="volume" bind:value={volume} type="range"/>
                        </FormGroup>
                    </Col>
                </Row>
                <Row>
                    <Col>
                        <FormGroup>
                            <Button on:click={play} color="primary">Play</Button>
                        </FormGroup>
                    </Col>
                </Row>
            </CardBody>
			<CardFooter>
				<Row>
					<Col>
						Pitch: {pitch} | Speed: {rate} | Volume: {volume} | voice: {printVoice(selectedVoice)}
					</Col>
				</Row>
			</CardFooter>
        </Card>
    </Container>
</main>

<style>

</style>