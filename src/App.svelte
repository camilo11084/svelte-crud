<script>
	import {v4} from 'uuid';
	import Noty from 'noty';
	
	import 'noty/lib/noty.css';
	import 'noty/lib/themes/sunset.css';

	

	let products = [
		{
			id:v4(),
			name: 'Dell',
			description: 'portatil Dell',
			category:'laptop'

		},
		{
			id:v4(),
			name: 'Mouse Razer',
			description: 'Gaming mouse',
			category:'perifericos'

		},
	];

	let product = {
		id: "",
		name: "",
		description: "",
		category: "",
		imageURL: "",
	};

	let editStatus = false;

	const cleanProduct = ()=>{
		product = {
			id: "",
			name: "",
			description: "",
			category: "",
			imageURL: "",
		}
	};


	const updateProduct = () =>{
		let updateProduct = {
			name: product.name,
			description : product.description,
			id: product.id,
			imageURL: product.imageURL,
			category: product.category
		}

		const productIndex = products.findIndex(p=> p.id === product.id)
		products[productIndex] = updateProduct;
		cleanProduct();
		editStatus = false;
		new Noty({
			theme: 'sunset',
			type: 'success',
			timeout: 3000,
			text: 'product update Succesfully '
		}).show();
	};

	const addProduct = () =>{
		
		const newProduct ={
			id: v4(),
			name: product.name,
			description: product.description,
			category: product.category,
			imageURL: product.imageURL
		}

		products = products.concat(newProduct)
		cleanProduct();
		console.log(products)
	};

	const onSubmitHandler = (e) => {
		if(!editStatus){
			addProduct();
		} else{
			updateProduct();
		}
		
	};


	const deleteProduct = (id)=>{
		products = products.filter(product => product.id !== id);
	};

	const editProduct = productEdit =>{
		product = productEdit;
		editStatus = true;
	};

</script>

<style>
</style>

<main>
	<div class="container p-4">
		<div class="row">
			<div class="col-md-6" >
				{#each products as product }
					<div class="card mt-2">
						<div class="row">

							<div class="col-md-4">
								{#if !product.imageURL}
									 <!-- content here -->
									 <img src="images/no_product.png" alt="" class="img-fluid p-2">
								{:else}
									<img src="{product.imageURL}" alt="" class="img-fluid p-2">

								{/if}
							</div>

							<div class="col-md-8">
								<div class="car-body">
									<h5>
										<strong>
											{product.name}
										</strong>
									</h5>
								</div>
								<p class="card-text">{product.description}</p>
								<button class="btn btn-danger" on:click={deleteProduct(product.id)}> Delete </button>
								<button class="btn btn-secondary" on:click={editProduct(product)}>Edit</button>
							</div>

						</div>
					</div>
				{/each}
			</div>

			<div class="col-md-6">
				<div class="card">
					<div class="card-body">
						<form on:submit|preventDefault={onSubmitHandler}>
							<div class="form-group">
								<input
									bind:value={product.name}
									type="text"
									placeholder="product name"
									id="product-name"
									class="form-control" />
							</div>

							<div class="form-group">
								<textarea
									bind:value={product.description}
									id="product-description"
									rows="3"
									class="form-control" />
							</div>

							<div class="form-group">
								<input
									bind:value={product.imageURL}
									type="url"
									id="product-image-url"
									placeholder="htpps:zentom.com"
									class="form-control" />
							</div>

							<div class="form-group">
								<select
									id="category"
									bind:value={product.category}
									class="form-control">
									<option value="laptops">Laptops</option>
									<option value="perifericos">
										Perifericos
									</option>
									<option value="servers">Servers</option>
								</select>
							</div>

							<button class="btn btn-secondary">
								{#if !editStatus}
									 <!-- content here -->
									 save product
								{:else}
									 <!-- else content here -->
									 update product
								{/if}	  
							</button>
						</form>
					</div>
				</div>
			</div>
		</div>
	</div>
</main>
