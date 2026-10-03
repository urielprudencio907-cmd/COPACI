<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistema de Control Ciudadano - COPACI</title>

    <style>
        * {
            box-sizing: border-box;
        }

        body {
            margin: 0;
            font-family: Arial, sans-serif;
            background: #e9fffc;
            color: #164e63;
        }

        header {
            background: #08a6a6;
            color: white;
            padding: 20px;
            text-align: center;
        }

        header h1 {
            margin: 0 0 8px;
        }

        header p {
            margin: 0;
        }

        nav {
            background: #087f8c;
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
        }

        nav button {
            border: none;
            background: transparent;
            color: white;
            padding: 15px 20px;
            cursor: pointer;
            font-size: 15px;
        }

        nav button:hover,
        nav button.activo {
            background: #05636e;
        }

        main {
            max-width: 1100px;
            margin: 25px auto;
            padding: 0 15px;
        }

        .seccion {
            display: none;
        }

        .seccion.activa {
            display: block;
        }

        .tarjetas {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 18px;
            margin-bottom: 25px;
        }

        .tarjeta {
            background: white;
            padding: 22px;
            border-radius: 10px;
            box-shadow: 0 3px 10px rgba(0,0,0,.08);
            text-align: center;
        }

        .tarjeta h3 {
            margin-top: 0;
            color: #08a6a6;
        }

        .numero {
            font-size: 32px;
            font-weight: bold;
        }

        .panel {
            background: white;
            padding: 22px;
            border-radius: 10px;
            box-shadow: 0 3px 10px rgba(0,0,0,.08);
            margin-bottom: 20px;
        }

        h2 {
            color: #08a6a6;
            margin-top: 0;
        }

        form {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 15px;
        }

        .campo {
            display: flex;
            flex-direction: column;
        }

        .campo.completo {
            grid-column: 1 / -1;
        }

        label {
            margin-bottom: 6px;
            font-weight: bold;
        }

        input, select, textarea {
            padding: 11px;
            border: 1px solid #9ddbd8;
            border-radius: 6px;
            font-size: 15px;
        }

        textarea {
            resize: vertical;
            min-height: 90px;
        }

        .boton {
            background: #08a6a6;
            color: white;
            border: none;
            padding: 12px 18px;
            border-radius: 6px;
            cursor: pointer;
            font-size: 15px;
        }

        .boton:hover {
            background: #087f8c;
        }

        .boton-rojo {
            background: #d95d6a;
        }

        .boton-rojo:hover {
            background: #b84452;
        }

        .tabla-contenedor {
            overflow-x: auto;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 15px;
        }

        th, td {
            border: 1px solid #bfe5e2;
            padding: 10px;
            text-align: left;
        }

        th {
            background: #08a6a6;
            color: white;
        }

        tr:nth-child(even) {
            background: #f1fffd;
        }

        .estado {
            padding: 5px 8px;
            border-radius: 5px;
            font-size: 13px;
            font-weight: bold;
        }

        .pendiente {
            background: #d9f7f5;
            color: #0b6f70;
        }

        .atendido {
            background: #d8f8f4;
            color: #087f7b;
        }

        footer {
            background: #087f8c;
            color: white;
            text-align: center;
            padding: 18px;
            margin-top: 30px;
        }

        .mensaje {
            display: none;
            padding: 12px;
            margin-bottom: 15px;
            background: #d8f8f4;
            color: #087f7b;
            border-radius: 6px;
        }

        @media (max-width: 700px) {
            form {
                grid-template-columns: 1fr;
            }

            .campo.completo {
                grid-column: auto;
            }

            nav button {
                width: 50%;
            }
        }
    </style>
</head>

<body>

<header>
    <h1>Sistema de Control Ciudadano</h1>
    <p>COPACI - Consejo de Participación Ciudadana</p>
</header>

<nav>
    <button class="activo" onclick="mostrarSeccion('inicio', this)">Inicio</button>
    <button onclick="mostrarSeccion('ciudadanos', this)">Ciudadanos</button>
    <button onclick="mostrarSeccion('reportes', this)">Reportes</button>
    <button onclick="mostrarSeccion('eventos', this)">Eventos</button>
    <button onclick="mostrarSeccion('consultas', this)">Consultas</button>
</nav>

<main>

    <!-- INICIO -->
    <section id="inicio" class="seccion activa">
        <div class="tarjetas">
            <div class="tarjeta">
                <h3>Ciudadanos</h3>
                <div class="numero" id="totalCiudadanos">0</div>
            </div>

            <div class="tarjeta">
                <h3>Reportes</h3>
                <div class="numero" id="totalReportes">0</div>
            </div>

            <div class="tarjeta">
                <h3>Pendientes</h3>
                <div class="numero" id="totalPendientes">0</div>
            </div>

            <div class="tarjeta">
                <h3>Eventos</h3>
                <div class="numero" id="totalEventos">0</div>
            </div>
        </div>

        <div class="panel">
            <h2>Bienvenido</h2>
            <p>
                Este sistema permite al COPACI llevar un control básico de
                ciudadanos, reportes comunitarios y eventos.
            </p>
            <p>
                Utiliza el menú superior para registrar y consultar información.
            </p>
        </div>
    </section>

    <!-- CIUDADANOS -->
    <section id="ciudadanos" class="seccion">
        <div class="panel">
            <h2>Registrar ciudadano</h2>

            <div id="mensajeCiudadano" class="mensaje">
                Ciudadano registrado correctamente.
            </div>

            <form id="formCiudadano">
                <div class="campo">
                    <label>Nombre completo:</label>
                    <input type="text" id="nombreCiudadano" required>
                </div>

                <div class="campo">
                    <label>Teléfono:</label>
                    <input type="tel" id="telefonoCiudadano" required>
                </div>

                <div class="campo">
                    <label>Correo electrónico:</label>
                    <input type="email" id="correoCiudadano">
                </div>

                <div class="campo">
                    <label>Colonia:</label>
                    <input type="text" id="coloniaCiudadano" required>
                </div>

                <div class="campo completo">
                    <button class="boton" type="submit">
                        Registrar ciudadano
                    </button>
                </div>
            </form>
        </div>

        <div class="panel">
            <h2>Lista de ciudadanos</h2>

            <div class="tabla-contenedor">
                <table>
                    <thead>
                        <tr>
                            <th>ID</th>
                            <th>Nombre</th>
                            <th>Teléfono</th>
                            <th>Correo</th>
                            <th>Colonia</th>
                            <th>Acción</th>
                        </tr>
                    </thead>
                    <tbody id="tablaCiudadanos"></tbody>
                </table>
            </div>
        </div>
    </section>

    <!-- REPORTES -->
    <section id="reportes" class="seccion">
        <div class="panel">
            <h2>Registrar reporte ciudadano</h2>

            <div id="mensajeReporte" class="mensaje">
                Reporte registrado correctamente.
            </div>

            <form id="formReporte">
                <div class="campo">
                    <label>Nombre del ciudadano:</label>
                    <input type="text" id="nombreReporte" required>
                </div>

                <div class="campo">
                    <label>Tipo de reporte:</label>
                    <select id="tipoReporte" required>
                        <option value="">Seleccionar</option>
                        <option>Alumbrado público</option>
                        <option>Basura</option>
                        <option>Seguridad</option>
                        <option>Agua</option>
                        <option>Calles y baches</option>
                        <option>Áreas verdes</option>
                        <option>Otro</option>
                    </select>
                </div>

                <div class="campo completo">
                    <label>Descripción:</label>
                    <textarea id="descripcionReporte" required></textarea>
                </div>

                <div class="campo">
                    <label>Fecha:</label>
                    <input type="date" id="fechaReporte" required>
                </div>

                <div class="campo">
                    <label>Estado:</label>
                    <select id="estadoReporte">
                        <option>Pendiente</option>
                        <option>Atendido</option>
                    </select>
                </div>

                <div class="campo completo">
                    <button class="boton" type="submit">
                        Registrar reporte
                    </button>
                </div>
            </form>
        </div>

        <div class="panel">
            <h2>Reportes registrados</h2>

            <div class="tabla-contenedor">
                <table>
                    <thead>
                        <tr>
                            <th>ID</th>
                            <th>Ciudadano</th>
                            <th>Tipo</th>
                            <th>Descripción</th>
                            <th>Fecha</th>
                            <th>Estado</th>
                            <th>Acción</th>
                        </tr>
                    </thead>
                    <tbody id="tablaReportes"></tbody>
                </table>
            </div>
        </div>
    </section>

    <!-- EVENTOS -->
    <section id="eventos" class="seccion">
        <div class="panel">
            <h2>Registrar evento o reunión</h2>

            <div id="mensajeEvento" class="mensaje">
                Evento registrado correctamente.
            </div>

            <form id="formEvento">
                <div class="campo">
                    <label>Nombre del evento:</label>
                    <input type="text" id="nombreEvento" required>
                </div>

                <div class="campo">
                    <label>Fecha:</label>
                    <input type="date" id="fechaEvento" required>
                </div>

                <div class="campo">
                    <label>Hora:</label>
                    <input type="time" id="horaEvento" required>
                </div>

                <div class="campo">
                    <label>Lugar:</label>
                    <input type="text" id="lugarEvento" required>
                </div>

                <div class="campo completo">
                    <button class="boton" type="submit">
                        Registrar evento
                    </button>
                </div>
            </form>
        </div>

        <div class="panel">
            <h2>Eventos registrados</h2>

            <div class="tabla-contenedor">
                <table>
                    <thead>
                        <tr>
                            <th>ID</th>
                            <th>Evento</th>
                            <th>Fecha</th>
                            <th>Hora</th>
                            <th>Lugar</th>
                            <th>Acción</th>
                        </tr>
                    </thead>
                    <tbody id="tablaEventos"></tbody>
                </table>
            </div>
        </div>
    </section>

    <!-- CONSULTAS -->
    <section id="consultas" class="seccion">
        <div class="panel">
            <h2>Consulta general</h2>

            <label>Buscar:</label>
            <input
                type="text"
                id="busqueda"
                placeholder="Escribe un nombre, reporte o colonia..."
                oninput="buscarInformacion()"
            >

            <div class="tabla-contenedor">
                <table>
                    <thead>
                        <tr>
                            <th>Tipo</th>
                            <th>Información</th>
                            <th>Fecha</th>
                            <th>Estado</th>
                        </tr>
                    </thead>
                    <tbody id="tablaConsultas"></tbody>
                </table>
            </div>
        </div>
    </section>

</main>

<footer>
    <p>Sistema de Control Ciudadano - COPACI</p>
    <p>© 2026</p>
</footer>

<script>
    let ciudadanos = JSON.parse(localStorage.getItem("ciudadanos")) || [];
    let reportes = JSON.parse(localStorage.getItem("reportes")) || [];
    let eventos = JSON.parse(localStorage.getItem("eventos")) || [];

    function mostrarSeccion(id, boton) {
        document.querySelectorAll(".seccion").forEach(seccion => {
            seccion.classList.remove("activa");
        });

        document.getElementById(id).classList.add("activa");

        document.querySelectorAll("nav button").forEach(b => {
            b.classList.remove("activo");
        });

        boton.classList.add("activo");

        if (id === "consultas") {
            buscarInformacion();
        }
    }

    function guardarDatos() {
        localStorage.setItem("ciudadanos", JSON.stringify(ciudadanos));
        localStorage.setItem("reportes", JSON.stringify(reportes));
        localStorage.setItem("eventos", JSON.stringify(eventos));
    }

    function actualizarInicio() {
        document.getElementById("totalCiudadanos").textContent = ciudadanos.length;
        document.getElementById("totalReportes").textContent = reportes.length;
        document.getElementById("totalPendientes").textContent =
            reportes.filter(r => r.estado === "Pendiente").length;
        document.getElementById("totalEventos").textContent = eventos.length;
    }

    function mostrarMensaje(id) {
        const mensaje = document.getElementById(id);
        mensaje.style.display = "block";

        setTimeout(() => {
            mensaje.style.display = "none";
        }, 2500);
    }

    document.getElementById("formCiudadano").addEventListener("submit", function(e) {
        e.preventDefault();

        ciudadanos.push({
            id: Date.now(),
            nombre: document.getElementById("nombreCiudadano").value,
            telefono: document.getElementById("telefonoCiudadano").value,
            correo: document.getElementById("correoCiudadano").value,
            colonia: document.getElementById("coloniaCiudadano").value
        });

        guardarDatos();
        mostrarCiudadanos();
        actualizarInicio();
        mostrarMensaje("mensajeCiudadano");
        this.reset();
    });

    function mostrarCiudadanos() {
        const tabla = document.getElementById("tablaCiudadanos");
        tabla.innerHTML = "";

        ciudadanos.forEach(c => {
            tabla.innerHTML += `
                <tr>
                    <td>${c.id}</td>
                    <td>${c.nombre}</td>
                    <td>${c.telefono}</td>
                    <td>${c.correo}</td>
                    <td>${c.colonia}</td>
                    <td>
                        <button class="boton boton-rojo"
                            onclick="eliminarCiudadano(${c.id})">
                            Eliminar
                        </button>
                    </td>
                </tr>
            `;
        });
    }

    function eliminarCiudadano(id) {
        if (confirm("¿Deseas eliminar este ciudadano?")) {
            ciudadanos = ciudadanos.filter(c => c.id !== id);
            guardarDatos();
            mostrarCiudadanos();
            actualizarInicio();
        }
    }

    document.getElementById("formReporte").addEventListener("submit", function(e) {
        e.preventDefault();

        reportes.push({
            id: Date.now(),
            ciudadano: document.getElementById("nombreReporte").value,
            tipo: document.getElementById("tipoReporte").value,
            descripcion: document.getElementById("descripcionReporte").value,
            fecha: document.getElementById("fechaReporte").value,
            estado: document.getElementById("estadoReporte").value
        });

        guardarDatos();
        mostrarReportes();
        actualizarInicio();
        buscarInformacion();
        mostrarMensaje("mensajeReporte");
        this.reset();
    });

    function mostrarReportes() {
        const tabla = document.getElementById("tablaReportes");
        tabla.innerHTML = "";

        reportes.forEach(r => {
            const clase = r.estado === "Pendiente" ? "pendiente" : "atendido";

            tabla.innerHTML += `
                <tr>
                    <td>${r.id}</td>
                    <td>${r.ciudadano}</td>
                    <td>${r.tipo}</td>
                    <td>${r.descripcion}</td>
                    <td>${r.fecha}</td>
                    <td>
                        <span class="estado ${clase}">
                            ${r.estado}
                        </span>
                    </td>
                    <td>
                        <button class="boton boton-rojo"
                            onclick="eliminarReporte(${r.id})">
                            Eliminar
                        </button>
                    </td>
                </tr>
            `;
        });
    }

    function eliminarReporte(id) {
        if (confirm("¿Deseas eliminar este reporte?")) {
            reportes = reportes.filter(r => r.id !== id);
            guardarDatos();
            mostrarReportes();
            actualizarInicio();
            buscarInformacion();
        }
    }

    document.getElementById("formEvento").addEventListener("submit", function(e) {
        e.preventDefault();

        eventos.push({
            id: Date.now(),
            nombre: document.getElementById("nombreEvento").value,
            fecha: document.getElementById("fechaEvento").value,
            hora: document.getElementById("horaEvento").value,
            lugar: document.getElementById("lugarEvento").value
        });

        guardarDatos();
        mostrarEventos();
        actualizarInicio();
        buscarInformacion();
        mostrarMensaje("mensajeEvento");
        this.reset();
    });

    function mostrarEventos() {
        const tabla = document.getElementById("tablaEventos");
        tabla.innerHTML = "";

        eventos.forEach(e => {
            tabla.innerHTML += `
                <tr>
                    <td>${e.id}</td>
                    <td>${e.nombre}</td>
                    <td>${e.fecha}</td>
                    <td>${e.hora}</td>
                    <td>${e.lugar}</td>
                    <td>
                        <button class="boton boton-rojo"
                            onclick="eliminarEvento(${e.id})">
                            Eliminar
                        </button>
                    </td>
                </tr>
            `;
        });
    }

    function eliminarEvento(id) {
        if (confirm("¿Deseas eliminar este evento?")) {
            eventos = eventos.filter(e => e.id !== id);
            guardarDatos();
            mostrarEventos();
            actualizarInicio();
            buscarInformacion();
        }
    }

    function buscarInformacion() {
        const texto = document.getElementById("busqueda").value.toLowerCase();
        const tabla = document.getElementById("tablaConsultas");
        tabla.innerHTML = "";

        ciudadanos
            .filter(c =>
                c.nombre.toLowerCase().includes(texto) ||
                c.colonia.toLowerCase().includes(texto)
            )
            .forEach(c => {
                tabla.innerHTML += `
                    <tr>
                        <td>Ciudadano</td>
                        <td>${c.nombre} - ${c.colonia}</td>
                        <td>-</td>
                        <td>-</td>
                    </tr>
                `;
            });

        reportes
            .filter(r =>
                r.ciudadano.toLowerCase().includes(texto) ||
                r.tipo.toLowerCase().includes(texto) ||
                r.descripcion.toLowerCase().includes(texto)
            )
            .forEach(r => {
                tabla.innerHTML += `
                    <tr>
                        <td>Reporte</td>
                        <td>${r.tipo}: ${r.descripcion}</td>
                        <td>${r.fecha}</td>
                        <td>${r.estado}</td>
                    </tr>
                `;
            });

        eventos
            .filter(e =>
                e.nombre.toLowerCase().includes(texto) ||
                e.lugar.toLowerCase().includes(texto)
            )
            .forEach(e => {
                tabla.innerHTML += `
                    <tr>
                        <td>Evento</td>
                        <td>${e.nombre} - ${e.lugar}</td>
                        <td>${e.fecha} ${e.hora}</td>
                        <td>Programado</td>
                    </tr>
                `;
            });
    }

    actualizarInicio();
    mostrarCiudadanos();
    mostrarReportes();
    mostrarEventos();
    buscarInformacion();
</script>

</body>
</html>
