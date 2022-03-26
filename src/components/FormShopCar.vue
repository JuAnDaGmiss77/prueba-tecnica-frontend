<template>
    <div class="body">
        <div class="container-form sign-up row">
            <div class="col-lg-6"></div>
            <div class="col-lg-6 d-flex justify-content-md-end justify-content-sm-between">
                <form class="formulario">
                    <h1>¡Cotiza la tuya!</h1>
                    <div class="mb-3">
                        <label for="exampleInputEmail1" class="form-label">Selecciona un modelo</label>
                        <select class="form-select">
                            <option value>prueba</option>
                            <option value>prueba</option>
                        </select>
                    </div>
                    <div class="mb-3">
                        <label class="form-label">Nombre completo</label>
                        <input type="text" class="form-control" />
                    </div>

                    <div class="mb-3">
                        <label class="form-label">Email</label>
                        <input type="text" class="form-control" />
                    </div>

                    <div class="mb-3">
                        <label class="form-label">Numero celular</label>
                        <input type="number" class="form-control" />
                    </div>
                    <div class="mb-3 row">
                        <div class="col-6">
                            <label class="form-label">Departamento</label>
                            <select class="form-select">
                                <option value>prueba</option>
                                <option value>prueba</option>
                            </select>
                        </div>

                        <div class="col-6">
                            <label class="form-label">Ciudad</label>
                            <select class="form-select">
                                <option value>prueba</option>
                                <option value>prueba</option>
                            </select>
                        </div>
                    </div>
                    <div class="form-check mb-3">
                        <input class="form-check-input" type="checkbox" value id="flexCheckDefault" />
                        <label class="form-check-label" for="flexCheckDefault">Acepto la politica de <a href="">Tratamiento de datos personales</a></label>
                    </div>

                    <button type="submit" class="btn btn-dark">Enviar datos</button>
                </form>
            </div>
        </div>
    </div>
</template>

<style scoped>

.body {
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    background-image: url("../assets/img/Img_car.png");
    background-position: center;
    background-repeat: no-repeat;
    background-size: cover;
}

.container-form {
    width: 100%;
    height: 100%;
    transition: all 0.5s ease-out;
    padding-bottom: 10px;
    margin-top: 10px;
}

.formulario {
    width: 450px;
    padding: 1rem;
    margin: 2rem;
    background-color: rgb(240, 240, 240);
    border-radius: 7px;
}
@media (max-width: 768px) {
    .formulario {
    width: 100vh;
    height: 100%;
    padding: 1rem;
    margin: 0.5rem;
    background-color: rgb(240, 240, 240);
    border-radius: 7px;
}
}

h1 {
    margin-top: 10px;
    margin-bottom: 10px;
    font-size: 1.7rem;
    color: black;
}
.btn-dark {
    background-color: black;
    border-radius: 50px;
    width: 150px;
}
</style>