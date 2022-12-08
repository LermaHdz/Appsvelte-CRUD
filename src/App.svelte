<script>

	let empleados=[];
	let activado =true;
	let datosEmpleado={
		id:null,
		nombre:"",
		correo:""
	}

	let mostrarEmpleado =()=>{
	fetch('http://localhost/empleados/')
	.then(respuesta=>respuesta.json())
	.then((datosRespuesta)=>{
		empleados=datosRespuesta;
		datosEmpleado={
			id:null,
			nombre:"",
			correo:""
		}
		activado=true;
		console.log(empleados);
	}).catch(console.log)
	}

	let agregarEmpleado=()=>{

		const nuevoEmpleado ={
			id: datosEmpleado.id,
			nombre: datosEmpleado.nombre,
			correo: datosEmpleado.correo
		} 
		
		fetch('http://localhost/empleados/?insertar=1',{
			method:"POST",
			body:JSON.stringify(nuevoEmpleado)
		})
		.then(respuesta=>respuesta.json())
		.then((datosRespuesta)=>{
			console.log(datosRespuesta);
	    	mostrarEmpleado();
	}).catch(console.log)
	}



	let borrarEmpleado =id =>{
		fetch('http://localhost/empleados/?borrar=' +id)
		.then(respuesta=>respuesta.json())
		.then((datosRespuesta)=>{
	    	mostrarEmpleado();
	}).catch(console.log)

	}

	let editarEmpleado = empleado =>{
		activado = false;
		datosEmpleado = empleado;

	}

	let actualizarEmpleado = empleado =>{

		fetch('http://localhost/empleados/?actualizar='+datosEmpleado.id,{
			method:"POST",
			body:JSON.stringify(datosEmpleado)
		})
		.then(respuesta=>respuesta.json())
		.then((datosRespuesta)=>{
			console.log(datosRespuesta);
	    	mostrarEmpleado();
	}).catch(console.log)

	

	}

	
mostrarEmpleado();
	
</script>

<div class="container">
	<div class="row">
		<div class="col-md-5">

			<div class="card">
				<div class="card-header">
					Empleados
				</div>
				<div class="card-body">
					<form action="" method="post">
						<div class="form-group">
						  <label for="">ID</label>
						  <input readonly
						  bind:value={datosEmpleado.id}
						  type="text" class="form-control" name="" id="" aria-describedby="helpId" placeholder="">
						</div>

						<div class="form-group">
							<label for="">Nombre</label>
							<input
							 bind:value={datosEmpleado.nombre}
							 type="text" class="form-control" name="" id="" aria-describedby="helpId" placeholder="">
						  </div>

						  <div class="form-group">
							<label for="">Correo</label>
							<input
							bind:value={datosEmpleado.correo}
							 type="text" class="form-control" name="" id="" aria-describedby="helpId" placeholder="">
						  </div>
						  <br>
						  <button type="button" disabled={!activado} class="btn btn-success" on:click|preventDefault={agregarEmpleado}>Agregar Empleado</button>
						  <button type="button" class="btn btn-primary" disabled={activado} on:click|preventDefault={actualizarEmpleado}>Actualizar</button>
						  <button type="button" class="btn btn-danger" disabled={activado} on:click|preventDefault={mostrarEmpleado}>Cancelar</button>
					</form>
				</div>

			</div>

		</div>

		<div class="col-md-7">
			<div class="table-responsive">
				<table class="table">
					<thead>
						<tr>
							<th >ID</th>
							<th >Nombre</th>
							<th >Correo</th>
							<th >Acciones</th>
						</tr>
					</thead>
					<tbody>

						{#each empleados as empleado}
						<tr>
							<td>{empleado.id}</td>
							<td>{empleado.nombre}</td>
							<td>{empleado.correo}</td>
							<td>
								<button type="submit" class="btn btn-primary"on:click={editarEmpleado(empleado)}> Editar</button>
								 | 
								<button type="submit" class="btn btn-danger" on:click={borrarEmpleado(empleado.id)}> Borrar</button>
							
							</td>
						</tr>

						{/each}
					</tbody>
				</table>
			</div>
		</div>
	</div>
</div>



