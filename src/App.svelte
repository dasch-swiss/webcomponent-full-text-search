<svelte:options tag="full-text-search"/>
<script>
	/**
	 * This component is used to create a webcomponent. Therefore all variables installed with export let can be used to give the webcomponent input.
	 * The output of the component is stored in the url variable, which can be read from outside the component.
	 */
	export let host = '0.0.0.0:3333'; //Defaults to local dsp-api
	export let project, restype, standoff; //Filters that can only be provided from outside the component. Have to be in iri format.
	let searchString;
	export let url; //export so it is readable from outside the component
	/**
	 * Creates the url (which contains the query) for viewer components to read. Use with HTTP GET!
	 */
	function search(){
		url = 'http://' + host + '/v2/search/' + encodeURIComponent(searchString);
		let currId = '?'
		if (restype){ //resource type iri was provided from outside the component
			url += currId + 'limitToResourceClass=' + encodeURIComponent(restype);
			currId = '&';
		}
		if (project){ //project iri was provided from outside the component
			url += currId + 'limitToProject=' + encodeURIComponent(project);
			currId = '&';
		}
		if (standoff){
			url += currId + 'limitToStandoffClass=' + encodeURIComponent(standoff);
			currId = '&';
		}
		//The following lines are used for testing, if you need the request to be sent.
		/*
		fetch(url, {
			method: 'GET'
		}).then(
			(res) => {
				res.json().then(
					(j) => {
						console.log(j);
					}
				)
			}
		);
		*/
	}
	/**
	 * Call with parameter, so Svelte knows to resend the request for the button. Checks whether the input is valid.
	 * @param s the string to check
	 * @return boolean
	 */
	function inputInvalid(s) {
		return !s;
	}
</script>

<div class="container">
	<div class="search">
		<input class="searchvalue" bind:value={searchString} placeholder="Enter search string">
		<button class="searchbutton" disabled='{inputInvalid(searchString)}' on:click={search}>Search</button>
	</div>
</div>

<style>
	.search {
		width:100%;
		position:relative;
		display:flex;
		height: 100%;
		}
	.searchvalue {
		width:100%;
		border: 3px solid;
		padding: 5px;
	}
	.searchbutton {
		width:10%;
		text-align: center;
		cursor:pointer;
	}
	.container {
		width: 40%;
		height: 3%;
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
	}
</style>