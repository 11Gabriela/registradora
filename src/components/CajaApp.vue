<template>
    <!--Título-->
    <div class="row d-flex justify-content-center mt-3">
        <div class="col-sm-4 d-flex justify-content-center">
            <h1 class="text-primary">
                {{ nombreCliente }}
            </h1>
        </div>
    </div>
    <!--Contenido-->
    <div class="row">
        <!--Formulario-->
        <div class="col-sm-6 p-5">
            <div>
                <div class="mb-3">
                    <label class="form-label">Nombre</label>
                    <input v-model="nombreCliente" type="text" class="form-control">
                </div>
                <div class="mb-3">
                    <label class="form-label">Producto</label>
                    <select v-model="idSeleccionado" @change="comprobar()" type="text" class="form-control">
                        <option value="0">-- Seleccionee --</option>
                        <option v-for="producto in productos" :value="producto.id">
                            {{ producto.nombre }}
                        </option>
                    </select>
                </div>
                <div class="mb-3">
                    <label class="form-label">Cantidad</label>
                    <input v-model="cantidad" type="number" class="form-control">
                </div>
                <div class="mb-3">
                    <button @click="agregarProducto()" class="btn btn-primary">
                        Agregar
                    </button>
                </div>
            </div>
        </div>
        <!--Resultado-->
        <div class="col-sm-6 mt-5 p-5">
            <table class="table table-bordered">
                <thead>
                    <tr>
                        <td>Producto</td>
                        <td>Cantidad</td>
                        <td>Valorr</td>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="itemProducto in listaCompra">
                        <td>{{ itemProducto.nombre }}</td>
                        <td align="right">{{ itemProducto.cantidad }}</td>
                        <td align="right">{{ itemProducto.valor }}</td>
                    </tr>
                    <tr>
                        <td>Subtotal</td>
                        <td colspan="2">{{ subtotal }}</td>
                    </tr>
                    <tr>
                        <td>IVA</td>
                        <td colspan="2">{{ iva }}</td>
                    </tr>
                    <tr>
                        <td>TOTAL</td>
                        <td colspan="2">{{ total }}</td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</template>
<script>
export default {
    name: 'CajaApp',
    data() {
        return {
            productos: [
                {
                    id: 1,
                    nombre: 'Arroz',
                    valor: 5000
                },
                {
                    id: 2,
                    nombre: 'Aceite',
                    valor: 10000
                },
                {
                    id: 3,
                    nombre: 'Huevos',
                    valor: 15000
                },
                {
                    id: 4,
                    nombre: 'Papel higiénico',
                    valor: 3000
                }
            ],
            idSeleccionado: 0,
            nombreCliente: '',
            cantidad: 1,
            listaCompra: []
        }
    },
    computed: {
        seleccionado() {
            if(this.idSeleccionado != 0){
                return this.productos.find((element) => element.id == this.idSeleccionado)
            }
            return false
        },
        subtotal() {
            return this.listaCompra.reduce((acc, item) => acc + item.valor, 0)
        },
        iva() {
            return this.subtotal * 0.19;
        },
        total() {
            return this.subtotal + this.iva;
        }
    }, 
    methods: {
        comprobar() {
            console.log('seleccionado', this.seleccionado)
        },
        agregarProducto() {
            if (this.seleccionado) {
                const productoExiste = this.listaCompra.find(item => item.nombre === this.seleccionado.nombre)
                if (productoExiste) {
                    productoExiste.cantidad += parseInt(this.cantidad);
                    productoExiste.valor = parseFloat(productoExiste.cantidad) * parseFloat(this.seleccionado.valor)
                } else {
                    this.listaCompra.push({
                        nombre: this.seleccionado.nombre,
                        cantidad: parseInt(this.cantidad),
                        valor: parseFloat(this.cantidad) * parseFloat(this.seleccionado.valor)
                    });
                 }
                 this.idSeleccionado = 0
                 this.cantidad = 1
            } else {
                alert('Debe seleccionar un producto')
            }
            console.log(this.listaCompra)
        }
    }
}
</script>
<style>
.table{
    text-align: right;
}
</style>