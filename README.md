# data-analist-en-spanish-
Tips de cómo ser analista de datos 
import React, { useState, useEffect, useMemo } from "react";

/* ══════════════════════════════════════════════════════════
   RENGLÓN · Curso de analista de datos
   Dirección visual: papel de contabilidad de barra verde
   (green-bar paper): renglones alternados, hilos finos,
   perforación lateral y tinta viridian. Todo el curso usa
   la misma base de datos: la tienda "La Higuera".
   ══════════════════════════════════════════════════════════ */

const TEMAS = {
  papel: {
    nombre: "Papel",
    bg: "#E9EDE2",
    panel: "#F4F6EF",
    stripe: "#E3EAD9",
    code: "#EDF1E6",
    rule: "#C6D0BB",
    ruleFaint: "#DBE3D2",
    ink: "#232E29",
    soft: "#54615A",
    faint: "#7E8B81",
    acc: "#1E6B5B",
    accSoft: "#D7E7DF",
    ocre: "#8A661C",
    ocreSoft: "#EFE6CF",
    num: "#3C5480",
    fn: "#6B4E8F",
    sombra: "rgba(35,46,41,0.07)",
  },
  noche: {
    nombre: "Noche",
    bg: "#12171A",
    panel: "#181F22",
    stripe: "#1D2528",
    code: "#161D20",
    rule: "#2C3639",
    ruleFaint: "#222B2E",
    ink: "#D2DAD5",
    soft: "#95A29B",
    faint: "#6F7D76",
    acc: "#63B79C",
    accSoft: "#1E2E2A",
    ocre: "#C6A45F",
    ocreSoft: "#252119",
    num: "#8AA6D6",
    fn: "#B199D6",
    sombra: "rgba(0,0,0,0.35)",
  },
};

const TAMANOS = [
  { id: "s", px: 16.5, et: "A", nota: "Cómoda" },
  { id: "m", px: 18.5, et: "A", nota: "Grande" },
  { id: "l", px: 20.5, et: "A", nota: "Extra" },
];

const SERIF =
  "'Iowan Old Style','Palatino Linotype','Book Antiqua',Palatino,Georgia,'Times New Roman',serif";
const SANS =
  "ui-sans-serif,-apple-system,'Segoe UI',Roboto,'Helvetica Neue',Arial,sans-serif";
const MONO =
  "ui-monospace,'SF Mono','JetBrains Mono',Menlo,Consolas,'Liberation Mono',monospace";

const CSS = `
.app *, .app *::before, .app *::after { box-sizing: border-box; }
.app {
  min-height: 100vh; background: var(--bg); color: var(--ink);
  font-family: var(--sans); -webkit-font-smoothing: antialiased;
  text-rendering: optimizeLegibility;
}
.app :focus-visible { outline: 2px solid var(--acc); outline-offset: 2px; border-radius: 2px; }
.app button { font: inherit; color: inherit; cursor: pointer; background: none; border: none; }

/* ── barra superior ── */
.top {
  position: sticky; top: 0; z-index: 30; background: var(--panel);
  border-bottom: 1px solid var(--rule);
}
.top-in {
  display: flex; align-items: center; gap: 14px;
  padding: 11px 20px; min-height: 56px; flex-wrap: wrap;
}
.marca { display: flex; align-items: baseline; gap: 9px; margin-right: auto; }
.marca b {
  font-family: var(--serif); font-size: 19px; font-weight: 600;
  letter-spacing: 0.01em;
}
.marca span {
  font-size: 10.5px; letter-spacing: 0.13em; text-transform: uppercase;
  color: var(--faint);
}
.ctrl { display: flex; align-items: center; gap: 6px; }
.pill {
  border: 1px solid var(--rule); border-radius: 999px; padding: 5px 12px;
  font-size: 12px; color: var(--soft); background: var(--bg);
  transition: background .15s, color .15s, border-color .15s;
}
.pill:hover { background: var(--accSoft); color: var(--acc); border-color: var(--acc); }
.pill.on { background: var(--accSoft); color: var(--acc); border-color: var(--acc); }
.sizer { display: flex; align-items: center; border: 1px solid var(--rule); border-radius: 999px; overflow: hidden; background: var(--bg); }
.sizer button { padding: 3px 10px; color: var(--faint); line-height: 1.6; }
.sizer button:hover { color: var(--acc); }
.sizer button.on { background: var(--accSoft); color: var(--acc); }
.avance { font-size: 11.5px; color: var(--faint); font-variant-numeric: tabular-nums; white-space: nowrap; }
.tally { height: 2px; background: var(--ruleFaint); }
.tally i { display: block; height: 100%; background: var(--acc); transition: width .4s ease; }

/* ── rejilla ── */
.wrap { display: grid; grid-template-columns: 296px minmax(0, 1fr); }
.wrap.solo { grid-template-columns: minmax(0, 1fr); }

/* ── índice lateral ── */
.rail {
  position: sticky; top: 58px; height: calc(100vh - 58px); overflow-y: auto;
  background: var(--panel); border-right: 1px solid var(--rule);
  padding: 18px 0 60px;
}
.rail h4 {
  margin: 0 18px 12px; font-size: 10.5px; letter-spacing: 0.13em;
  text-transform: uppercase; color: var(--faint); font-weight: 600;
}
.mod { border-top: 1px solid var(--ruleFaint); }
.mod:last-child { border-bottom: 1px solid var(--ruleFaint); }
.mod-b {
  display: flex; align-items: center; gap: 10px; width: 100%;
  padding: 11px 18px; text-align: left;
}
.mod-b:hover { background: var(--stripe); }
.mod-n {
  font-family: var(--mono); font-size: 11px; color: var(--faint);
  font-variant-numeric: tabular-nums;
}
.mod-t { font-size: 13.5px; line-height: 1.35; flex: 1; }
.mod-c { font-size: 10px; color: var(--faint); font-variant-numeric: tabular-nums; }
.ses-b {
  display: flex; gap: 9px; width: 100%; padding: 8px 18px 8px 40px;
  text-align: left; font-size: 12.5px; line-height: 1.4; color: var(--soft);
  border-left: 2px solid transparent;
}
.ses-b:nth-child(even) { background: var(--stripe); }
.ses-b:hover { color: var(--ink); }
.ses-b.aqui { border-left-color: var(--acc); background: var(--accSoft); color: var(--ink); font-weight: 600; }
.ses-b.hecha { color: var(--faint); }
.tic { width: 12px; flex: none; color: var(--acc); font-size: 11px; line-height: 1.5; }

/* ── lienzo de lectura ── */
.stage { display: flex; justify-content: center; padding: 46px 30px 140px; }
.paper { width: 100%; max-width: 700px; font-size: var(--fs); }
.paper.ancho { max-width: 760px; }
@keyframes entra { from { opacity: 0; transform: translateY(6px); } to { opacity: 1; transform: none; } }
.paper { animation: entra .24s ease both; }

.eyebrow {
  font-size: 10.5px; letter-spacing: 0.14em; text-transform: uppercase;
  color: var(--faint); margin-bottom: 14px; display: flex; gap: 9px; flex-wrap: wrap;
}
.eyebrow i { font-style: normal; color: var(--rule); }
.h1 {
  font-family: var(--serif); font-size: 1.72em; line-height: 1.22;
  font-weight: 600; letter-spacing: -0.012em; margin: 0 0 16px;
}
.obj {
  font-family: var(--serif); font-style: italic; font-size: 0.97em;
  line-height: 1.6; color: var(--soft); border-left: 2px solid var(--acc);
  padding: 2px 0 2px 14px; margin: 0 0 30px;
}
.p { font-family: var(--serif); font-size: 1em; line-height: 1.78; margin: 0 0 19px; }
.h2 {
  font-family: var(--sans); font-size: 0.7em; letter-spacing: 0.12em;
  text-transform: uppercase; color: var(--faint); font-weight: 600;
  margin: 34px 0 14px; padding-bottom: 7px; border-bottom: 1px solid var(--ruleFaint);
}
.ul { font-family: var(--serif); font-size: 1em; line-height: 1.7; margin: 0 0 19px; padding: 0; list-style: none; }
.ul li { position: relative; padding-left: 20px; margin-bottom: 9px; }
.ul li::before {
  content: ''; position: absolute; left: 2px; top: 0.72em; width: 7px; height: 1px;
  background: var(--acc);
}
.chip {
  font-family: var(--mono); font-size: 0.84em; background: var(--stripe);
  border: 1px solid var(--ruleFaint); border-radius: 2px; padding: 1px 5px;
  white-space: nowrap;
}

/* ── consulta ── */
.cap {
  font-size: 0.63em; letter-spacing: 0.12em; text-transform: uppercase;
  color: var(--faint); margin-bottom: 7px;
}
.code {
  font-family: var(--mono); font-size: 0.79em; line-height: 1.8;
  background: var(--code); border: 1px solid var(--rule); border-left: 2px solid var(--acc);
  border-radius: 2px; padding: 15px 18px; margin: 0 0 22px; overflow-x: auto;
  white-space: pre; tab-size: 2;
}
.k { color: var(--acc); font-weight: 600; }
.s { color: var(--ocre); }
.n { color: var(--num); }
.f { color: var(--fn); }
.c { color: var(--faint); font-style: italic; }

/* ── hoja de resultados (firma visual) ── */
.hoja {
  position: relative; border: 1px solid var(--rule); border-radius: 2px;
  background: var(--panel); margin: 0 0 24px; box-shadow: 0 1px 0 var(--sombra);
}
.hoja::before {
  content: ''; position: absolute; left: 0; top: 0; bottom: 0; width: 17px;
  background-color: var(--stripe);
  background-image: radial-gradient(circle, var(--bg) 2.4px, transparent 2.7px);
  background-size: 17px 23px; background-position: center 6px;
  border-right: 1px solid var(--ruleFaint); border-radius: 2px 0 0 2px;
}
.hoja-in { margin-left: 17px; overflow-x: auto; }
.hoja table { border-collapse: collapse; width: 100%; font-family: var(--mono); font-size: 0.76em; }
.hoja th {
  text-align: left; font-family: var(--sans); font-size: 9.5px; font-weight: 600;
  letter-spacing: 0.1em; text-transform: uppercase; color: var(--faint);
  padding: 9px 15px; border-bottom: 1px solid var(--rule); white-space: nowrap;
}
.hoja td { padding: 7px 15px; border-bottom: 1px solid var(--ruleFaint); white-space: nowrap; }
.hoja tbody tr:nth-child(even) { background: var(--stripe); }
.hoja tbody tr:last-child td { border-bottom: none; }
.hoja .nulo { color: var(--faint); font-style: italic; }
.hoja-pie {
  margin-left: 17px; padding: 7px 15px; border-top: 1px solid var(--ruleFaint);
  font-size: 10.5px; color: var(--faint); text-align: right;
  font-variant-numeric: tabular-nums;
}

/* ── marginalia ── */
.nota, .ej, .clave { margin: 0 0 22px; padding: 14px 17px; border-radius: 2px; font-family: var(--serif); line-height: 1.65; font-size: 0.94em; }
.nota { background: var(--panel); border: 1px solid var(--ruleFaint); border-left: 2px solid var(--rule); color: var(--soft); }
.ej { background: var(--panel); border: 1px dashed var(--rule); }
.clave { background: var(--ocreSoft); border-left: 2px solid var(--ocre); }
.et {
  display: block; font-family: var(--sans); font-size: 9.5px; font-weight: 600;
  letter-spacing: 0.14em; text-transform: uppercase; margin-bottom: 7px;
}
.ej .et { color: var(--acc); }
.clave .et { color: var(--ocre); }
.nota .et { color: var(--faint); }
.pista { display: block; margin-top: 10px; font-size: 0.88em; color: var(--faint); font-style: italic; }

/* ── pie de sesión ── */
.pie { margin-top: 44px; padding-top: 22px; border-top: 1px solid var(--rule); }
.marcar {
  display: flex; align-items: center; gap: 9px; width: 100%; justify-content: center;
  border: 1px solid var(--rule); border-radius: 2px; padding: 12px; font-size: 13px;
  color: var(--soft); background: var(--panel); transition: all .15s;
}
.marcar:hover { border-color: var(--acc); color: var(--acc); }
.marcar.on { background: var(--accSoft); border-color: var(--acc); color: var(--acc); }
.saltos { display: flex; gap: 10px; margin-top: 14px; }
.salto {
  flex: 1; border: 1px solid var(--ruleFaint); border-radius: 2px; padding: 12px 14px;
  text-align: left; background: var(--panel); transition: border-color .15s;
  min-width: 0;
}
.salto:hover { border-color: var(--acc); }
.salto.der { text-align: right; }
.salto small { display: block; font-size: 10px; letter-spacing: 0.12em; text-transform: uppercase; color: var(--faint); margin-bottom: 4px; }
.salto span { font-family: var(--serif); font-size: 14px; line-height: 1.35; display: block; overflow: hidden; text-overflow: ellipsis; }
.salto[disabled] { opacity: .35; cursor: default; }
.salto[disabled]:hover { border-color: var(--ruleFaint); }
.teclas { margin-top: 18px; text-align: center; font-size: 11px; color: var(--faint); }

/* ── portada de módulo ── */
.mod-head { margin-bottom: 34px; }
.mod-num {
  font-family: var(--mono); font-size: 0.72em; color: var(--acc);
  letter-spacing: 0.1em; margin-bottom: 10px;
}

.velo { display: none; }

@media (max-width: 900px) {
  .wrap { grid-template-columns: minmax(0, 1fr); }
  .rail {
    position: fixed; top: 58px; left: 0; width: 300px; max-width: 86vw; z-index: 40;
    transform: translateX(-101%); transition: transform .22s ease;
    box-shadow: 0 0 40px var(--sombra); height: calc(100vh - 58px);
  }
  .rail.abierto { transform: none; }
  .velo { display: block; position: fixed; inset: 58px 0 0; background: rgba(0,0,0,.28); z-index: 35; }
  .stage { padding: 30px 18px 120px; }
  .hoja::before { display: none; }
  .hoja-in, .hoja-pie { margin-left: 0; }
  .top-in { padding: 9px 14px; gap: 10px; }
  .marca span { display: none; }
  .saltos { flex-direction: column; }
  .salto.der { text-align: left; }
}
@media (prefers-reduced-motion: reduce) {
  .app *, .app *::before { animation: none !important; transition: none !important; }
}
`;

/* ══════════════════════════════════════════════════════════
   CONTENIDO DEL CURSO
   ══════════════════════════════════════════════════════════ */

const M1 = {
  id: "m1", n: "01",
  titulo: "Qué hace un analista de datos",
  resumen: "El oficio, el vocabulario y el lugar donde vas a practicar.",
  sesiones: [
    {
      id: "m1s1",
      titulo: "El trabajo real de un analista",
      objetivo: "Saber qué produces, para quién y con qué materia prima.",
      bloques: [
        { t: "p", x: "Un analista de datos convierte registros sueltos en decisiones. Nadie te va a pedir «una consulta SQL»: te van a pedir saber por qué cayeron las ventas del martes, qué clientes ya no vuelven o si conviene abrir reparto los domingos." },
        { t: "p", x: "El trabajo tiene cuatro movimientos que se repiten toda la carrera: **entender la pregunta**, **traerte el dato**, **limpiarlo y calcularlo**, y **contarlo** de forma que alguien pueda actuar. SQL vive en el segundo y tercer movimiento, y por eso es lo primero que vas a aprender aquí." },
        { t: "h", x: "Lo que entregas" },
        { t: "ul", x: [
          "Una respuesta con número y contexto, no un archivo crudo.",
          "Una métrica que alguien va a mirar cada semana (un tablero).",
          "Un hallazgo que cambia una decisión: subir un precio, cortar un producto, llamar a un cliente.",
        ] },
        { t: "nota", et: "Diferencia útil", x: "El analista explica lo que **ya pasó** y por qué. El científico de datos estima lo que **va a pasar**. Casi todo el valor de una empresa pequeña o mediana está en el primero, y se hace con SQL y una buena gráfica." },
        { t: "ej", x: "Escribe tres preguntas que te gustaría contestarle a un negocio que conozcas (la tienda de la esquina, tu propio canal, tu trabajo). No pienses todavía en cómo se resuelven.", pista: "Una pregunta buena tiene sujeto, periodo y comparación: «¿cuánto vendió la sucursal norte este mes contra el pasado?»." },
        { t: "clave", x: "Un dato sin decisión detrás es un pasatiempo. Empieza siempre por la pregunta." },
      ],
    },
    {
      id: "m1s2",
      titulo: "De la pregunta de negocio al dato",
      objetivo: "Traducir una frase vaga en algo medible.",
      bloques: [
        { t: "p", x: "«Queremos más clientes leales» no se puede consultar. Tu primer oficio es traducir. Traducir es elegir una **definición**, un **periodo** y un **criterio de corte**, y dejarlos por escrito antes de tocar el teclado." },
        { t: "h", x: "La traducción, paso a paso" },
        { t: "ul", x: [
          "Pregunta original: «¿tenemos clientes leales?»",
          "Definición: cliente con 3 o más pedidos entregados.",
          "Periodo: últimos 90 días.",
          "Métrica: cuántos son y qué porcentaje de la venta representan.",
        ] },
        { t: "p", x: "Ese párrafo ya es casi una consulta. Nota que tomaste decisiones discutibles (¿3 pedidos o 4?, ¿cuentan los cancelados?). Escribirlas es lo que separa un análisis defendible de uno que se cae en la primera junta." },
        { t: "nota", et: "Trampa clásica", x: "Si no fijas el periodo, cada quien lo calcula distinto y los números nunca cuadran entre áreas. La mitad de las discusiones de datos son discusiones de definiciones." },
        { t: "ej", x: "Toma una de tus tres preguntas de la sesión anterior y escríbele definición, periodo y criterio de corte." },
        { t: "clave", x: "Definición + periodo + corte. Sin esos tres, no hay número que valga." },
      ],
    },
    {
      id: "m1s3",
      titulo: "Cómo se guardan los datos: tablas, filas y columnas",
      objetivo: "Leer un esquema y saber dónde vive cada cosa.",
      bloques: [
        { t: "p", x: "Una base de datos relacional es un conjunto de tablas. Cada **tabla** guarda un tipo de cosa (clientes, pedidos, productos). Cada **fila** es una de esas cosas. Cada **columna** es un dato de esa cosa, siempre del mismo tipo: texto, número, fecha o booleano." },
        { t: "p", x: "Este curso entero usa la misma base: **La Higuera**, una tienda de abarrotes con reparto. Conócela ahora y no vuelvas a perder tiempo entendiendo datos de ejemplo." },
        { t: "h", x: "El esquema de La Higuera" },
        { t: "res",
          head: ["tabla", "una fila es…", "columnas principales"],
          rows: [
            ["clientes", "una persona", "id_cliente, nombre, ciudad, fecha_alta"],
            ["pedidos", "una compra", "id_pedido, id_cliente, fecha, total, estado"],
            ["detalle", "un renglón del ticket", "id_pedido, id_producto, cantidad, precio_unit"],
            ["productos", "un artículo", "id_producto, nombre, categoria, precio, activo"],
            ["empleados", "un trabajador", "id_empleado, nombre, id_jefe, sueldo, ciudad"],
          ],
          meta: "5 tablas · esquema fijo del curso" },
        { t: "p", x: "Fíjate en que `pedidos` guarda `id_cliente` en vez de repetir el nombre del cliente. Ese identificador es el hilo que une las tablas, y es la idea que hace posible todo lo demás." },
        { t: "ej", x: "Dibuja en papel las cinco tablas y traza una flecha de cada columna `id_algo` a la tabla donde ese id es único. Vas a acabar con un mapa: eso es un diagrama entidad-relación." },
        { t: "clave", x: "Tabla = cosa. Fila = una cosa. Columna = un dato de esa cosa. Los `id_` son los hilos entre tablas." },
      ],
    },
    {
      id: "m1s4",
      titulo: "Tu entorno: dónde escribir SQL hoy mismo",
      objetivo: "Tener una base corriendo en menos de diez minutos.",
      bloques: [
        { t: "p", x: "No instales nada complicado todavía. Para aprender, cualquiera de estas tres opciones sirve y todas entienden el SQL de este curso:" },
        { t: "ul", x: [
          "**DB Fiddle** o **SQLime** en el navegador: pegas tablas y consultas, cero instalación. Ideal para las primeras semanas.",
          "**DBeaver** + **SQLite** en tu computadora: un archivo `.db` y un editor decente. Es lo más parecido al trabajo real.",
          "**PostgreSQL** local o en la nube: el motor que más vas a encontrar en vacantes. Déjalo para el módulo 6.",
        ] },
        { t: "p", x: "Crea tu primera tabla y mete tres filas. No necesitas entender la sintaxis todavía, solo comprobar que el motor responde." },
        { t: "sql", cap: "Prueba de vida",
          x: `CREATE TABLE clientes (
  id_cliente  INTEGER PRIMARY KEY,
  nombre      TEXT,
  ciudad      TEXT,
  fecha_alta  DATE
);

INSERT INTO clientes VALUES
  (1, 'Ana Rocha',      'Culiacán', '2025-01-14'),
  (2, 'Beto Salcido',   'Mazatlán', '2025-02-03'),
  (3, 'Cynthia Ibarra', 'Culiacán', '2025-02-27');

SELECT * FROM clientes;` },
        { t: "res", head: ["id_cliente", "nombre", "ciudad", "fecha_alta"],
          rows: [["1", "Ana Rocha", "Culiacán", "2025-01-14"], ["2", "Beto Salcido", "Mazatlán", "2025-02-03"], ["3", "Cynthia Ibarra", "Culiacán", "2025-02-27"]],
          meta: "3 filas · 0.004 s" },
        { t: "nota", et: "Hábito", x: "Guarda tus consultas en archivos `.sql` con un comentario arriba que diga qué contestan. En seis meses vas a reutilizar el 80% de lo que escribas." },
        { t: "clave", x: "Herramienta que ya funciona hoy le gana a la herramienta perfecta del mes que viene." },
      ],
    },
  ],
};

const M2 = {
  id: "m2", n: "02",
  titulo: "SQL: tus primeras consultas",
  resumen: "Pedir columnas, filtrar filas y ordenar resultados.",
  sesiones: [
    {
      id: "m2s1",
      titulo: "SELECT y FROM: pedir columnas",
      objetivo: "Traer exactamente las columnas que necesitas.",
      bloques: [
        { t: "p", x: "Una consulta responde dos cosas: **qué columnas quiero** (`SELECT`) y **de dónde salen** (`FROM`). Nada más. Todo lo demás que aprendas en este curso son capas encima de esas dos palabras." },
        { t: "sql", cap: "Todas las columnas", x: `SELECT * FROM productos;` },
        { t: "sql", cap: "Solo lo que necesitas", x: `SELECT nombre, categoria, precio
FROM productos;` },
        { t: "res", head: ["nombre", "categoria", "precio"],
          rows: [["Harina 1 kg", "Abarrotes", "32.50"], ["Café molido 500 g", "Bebidas", "128.00"], ["Jabón en barra", "Limpieza", "18.90"], ["Refresco 2 L", "Bebidas", "38.00"]],
          meta: "4 filas · 0.003 s" },
        { t: "p", x: "El asterisco es cómodo para explorar, pero en una consulta que vas a guardar es mala señal: trae columnas que no usas, se rompe cuando alguien cambia la tabla y hace más lento todo. Pide lo que necesitas." },
        { t: "nota", et: "Estilo", x: "Escribe las palabras de SQL en mayúsculas y los nombres de tus tablas en minúsculas. Nadie te obliga, pero cuando una consulta crece a treinta líneas vas a agradecer poder leerla de un vistazo." },
        { t: "ej", x: "Trae el nombre y la ciudad de todos los clientes. Después trae solo la columna `ciudad` y observa que se repiten valores: eso te va a importar en la sesión 6." },
        { t: "clave", x: "`SELECT` elige columnas, `FROM` elige la tabla. Evita `SELECT *` en consultas que vayan a durar." },
      ],
    },
    {
      id: "m2s2",
      titulo: "WHERE: filtrar filas",
      objetivo: "Quedarte solo con las filas que cumplen una condición.",
      bloques: [
        { t: "p", x: "`SELECT` recorta a lo ancho (columnas); `WHERE` recorta a lo alto (filas). La cláusula evalúa la condición fila por fila y conserva las que dan verdadero." },
        { t: "sql", cap: "Productos de más de 100 pesos", x: `SELECT nombre, precio
FROM productos
WHERE precio > 100;` },
        { t: "res", head: ["nombre", "precio"], rows: [["Café molido 500 g", "128.00"], ["Aceite 1 L", "112.00"]], meta: "2 filas · 0.002 s" },
        { t: "h", x: "Comparadores" },
        { t: "res", head: ["operador", "significa", "ejemplo"],
          rows: [["=", "igual a", "ciudad = 'Culiacán'"], ["<> o !=", "distinto de", "estado <> 'cancelado'"], ["> >= < <=", "mayor / menor", "total >= 500"], ["BETWEEN", "rango cerrado", "total BETWEEN 100 AND 500"]],
          meta: "4 operadores" },
        { t: "p", x: "El texto va entre comillas simples y **sí distingue mayúsculas** en la mayoría de los motores. Los números no llevan comillas. Las fechas se escriben como texto en formato `AAAA-MM-DD`." },
        { t: "nota", et: "Error frecuente", x: "`WHERE ciudad = 'culiacán'` no devuelve nada si en la tabla dice `'Culiacán'`. Si dudas, usa `LOWER(ciudad) = 'culiacán'` y lo resuelves." },
        { t: "ej", x: "Trae los pedidos con total mayor a 800 pesos. Después los que estén entre 200 y 400. Después los que **no** estén cancelados." },
        { t: "clave", x: "`WHERE` se ejecuta antes de que exista el resultado: filtra los ingredientes, no el platillo." },
      ],
    },
    {
      id: "m2s3",
      titulo: "Combinar condiciones: AND, OR, IN, LIKE",
      objetivo: "Filtrar por varias reglas sin equivocarte de paréntesis.",
      bloques: [
        { t: "p", x: "`AND` exige que se cumplan todas las condiciones; `OR` exige al menos una. Cuando mezclas los dos, `AND` se evalúa primero, y ahí es donde se pierden analistas enteros." },
        { t: "sql", cap: "El paréntesis cambia la respuesta", x: `-- Mal: trae TODO Mazatlán, sin importar el total
SELECT * FROM pedidos
WHERE ciudad = 'Culiacán' OR ciudad = 'Mazatlán' AND total > 500;

-- Bien: las dos ciudades, pero solo pedidos grandes
SELECT * FROM pedidos
WHERE (ciudad = 'Culiacán' OR ciudad = 'Mazatlán') AND total > 500;` },
        { t: "h", x: "Atajos que vas a usar todos los días" },
        { t: "ul", x: [
          "`IN ('Culiacán','Mazatlán','Los Mochis')` sustituye una cadena de `OR`.",
          "`NOT IN (...)` invierte esa lista (cuidado con los nulos, lo vemos en la próxima sesión).",
          "`LIKE 'Café%'` busca texto: `%` es cualquier cosa, `_` es un solo carácter.",
          "`BETWEEN '2026-01-01' AND '2026-01-31'` para un mes completo.",
        ] },
        { t: "sql", cap: "Búsqueda por texto", x: `SELECT nombre, categoria
FROM productos
WHERE nombre LIKE '%café%'
   OR categoria IN ('Bebidas', 'Abarrotes');` },
        { t: "ej", x: "Trae los clientes de Culiacán o Mazatlán dados de alta en 2026. Escríbelo primero con `OR` y luego con `IN`, y compara cuál te parece más legible dentro de seis meses." },
        { t: "clave", x: "Cuando mezcles `AND` y `OR`, pon paréntesis aunque no hagan falta. Le estás escribiendo a tu yo del futuro." },
      ],
    },
    {
      id: "m2s4",
      titulo: "NULL: la ausencia no es un cero",
      objetivo: "Dejar de perder filas sin darte cuenta.",
      bloques: [
        { t: "p", x: "`NULL` significa «aquí no hay valor». No es cero, no es cadena vacía, no es falso: es desconocido. Y lo desconocido contamina toda comparación: `NULL = NULL` **no** es verdadero, es desconocido." },
        { t: "sql", cap: "La forma correcta de preguntar", x: `-- Esto nunca devuelve filas:
SELECT * FROM clientes WHERE telefono = NULL;

-- Esto sí:
SELECT * FROM clientes WHERE telefono IS NULL;
SELECT * FROM clientes WHERE telefono IS NOT NULL;` },
        { t: "p", x: "También afecta a los filtros negativos. Si una fila tiene `estado = NULL`, la condición `estado <> 'cancelado'` la descarta, porque el motor no puede afirmar que sea distinta. Si quieres conservarla, dilo explícitamente." },
        { t: "sql", cap: "Rescatar los nulos y darles un valor", x: `SELECT nombre,
       COALESCE(telefono, 'sin registrar') AS telefono
FROM clientes
WHERE estado <> 'cancelado' OR estado IS NULL;` },
        { t: "res", head: ["nombre", "telefono"],
          rows: [["Ana Rocha", "667-112-4408"], ["Beto Salcido", "sin registrar"], ["Cynthia Ibarra", "669-330-1187"]],
          meta: "3 filas · COALESCE sustituyó 1 nulo" },
        { t: "nota", et: "Por qué importa", x: "La mayoría de los reportes que «no cuadran» no cuadran por nulos. Antes de entregar un número, pregúntate qué filas se quedaron fuera sin que lo pidieras." },
        { t: "ej", x: "Cuenta cuántos clientes tienen teléfono y cuántos no. Deben sumar el total de la tabla; si no suman, ya encontraste tu primer problema de calidad de datos." },
        { t: "clave", x: "`IS NULL`, nunca `= NULL`. Y todo filtro negativo necesita que decidas qué haces con los nulos." },
      ],
    },
    {
      id: "m2s5",
      titulo: "ORDER BY y LIMIT: los primeros de la lista",
      objetivo: "Responder «cuáles son los diez mejores» en una línea.",
      bloques: [
        { t: "p", x: "Sin `ORDER BY` no tienes ninguna garantía sobre el orden de las filas, aunque parezcan ordenadas. Si el orden importa, pídelo." },
        { t: "sql", cap: "Top 5 de pedidos", x: `SELECT id_pedido, id_cliente, total
FROM pedidos
WHERE estado = 'entregado'
ORDER BY total DESC
LIMIT 5;` },
        { t: "res", head: ["id_pedido", "id_cliente", "total"],
          rows: [["1042", "7", "2,480.00"], ["1017", "3", "1,905.50"], ["1088", "7", "1,730.00"], ["1003", "12", "1,512.25"], ["1061", "2", "1,344.00"]],
          meta: "5 filas · ordenado por total desc" },
        { t: "ul", x: [
          "`ASC` es ascendente y es el valor por omisión; `DESC` es descendente.",
          "Puedes ordenar por varias columnas: `ORDER BY ciudad ASC, total DESC`.",
          "`LIMIT 10 OFFSET 20` salta las primeras 20 filas: así se pagina.",
          "En SQL Server se escribe `SELECT TOP 5` en lugar de `LIMIT`.",
        ] },
        { t: "nota", et: "Ojo con los nulos", x: "Al ordenar, unos motores mandan los `NULL` al principio y otros al final. Si te importa, escribe `ORDER BY total DESC NULLS LAST` donde esté soportado." },
        { t: "ej", x: "Saca los 3 productos más caros de cada categoría… o inténtalo. Vas a descubrir que con lo que sabes hoy no se puede: eso se resuelve en el módulo 5 con funciones de ventana. Apúntalo." },
        { t: "clave", x: "Sin `ORDER BY` no hay orden. Con `LIMIT` sin `ORDER BY`, el resultado es aleatorio disfrazado." },
      ],
    },
    {
      id: "m2s6",
      titulo: "Alias, cálculos y DISTINCT",
      objetivo: "Crear columnas nuevas y quitar repetidos.",
      bloques: [
        { t: "p", x: "En el `SELECT` no solo pides columnas: puedes calcular. El resultado es una columna nueva que no existe en la tabla, y conviene bautizarla con `AS`." },
        { t: "sql", cap: "Columna calculada", x: `SELECT nombre,
       precio,
       ROUND(precio * 1.16, 2) AS precio_con_iva
FROM productos
ORDER BY precio_con_iva DESC;` },
        { t: "res", head: ["nombre", "precio", "precio_con_iva"],
          rows: [["Café molido 500 g", "128.00", "148.48"], ["Aceite 1 L", "112.00", "129.92"], ["Refresco 2 L", "38.00", "44.08"]],
          meta: "3 filas · columna calculada" },
        { t: "p", x: "`DISTINCT` elimina filas repetidas del resultado. Sirve para explorar qué valores existen realmente en una columna, que es siempre lo primero que debes hacer con una tabla nueva." },
        { t: "sql", cap: "¿Qué valores hay aquí dentro?", x: `SELECT DISTINCT estado FROM pedidos;
SELECT DISTINCT ciudad, categoria FROM ventas;  -- combinaciones únicas` },
        { t: "nota", et: "Cuidado", x: "`DISTINCT` aplica a la fila completa, no a una columna suelta. Si ves un `DISTINCT` puesto para «arreglar» filas duplicadas, casi siempre el problema real está en un `JOIN` mal hecho (módulo 4)." },
        { t: "ej", x: "Lista las ciudades distintas de la tabla `clientes` y compáralas: si aparecen «Culiacán» y «culiacan», acabas de encontrar trabajo de limpieza para el módulo 6." },
        { t: "clave", x: "El `SELECT` es una calculadora: alias claros hoy, reportes legibles siempre." },
      ],
    },
  ],
};

const M3 = {
  id: "m3", n: "03",
  titulo: "Agrupar y resumir",
  resumen: "De miles de filas a un número que significa algo.",
  sesiones: [
    {
      id: "m3s1",
      titulo: "Funciones de agregación",
      objetivo: "Convertir muchas filas en un solo valor.",
      bloques: [
        { t: "p", x: "Una función de agregación toma un montón de filas y devuelve uno. Son cinco y las vas a usar el resto de tu vida profesional." },
        { t: "res", head: ["función", "responde", "ignora nulos"],
          rows: [["COUNT(*)", "¿cuántas filas hay?", "no"], ["COUNT(col)", "¿cuántas tienen valor?", "sí"], ["SUM(col)", "¿cuánto suma?", "sí"], ["AVG(col)", "¿cuál es el promedio?", "sí"], ["MIN / MAX", "¿el menor / el mayor?", "sí"]],
          meta: "5 funciones" },
        { t: "sql", cap: "Radiografía de una tabla", x: `SELECT COUNT(*)          AS pedidos,
       SUM(total)        AS venta_total,
       ROUND(AVG(total), 2) AS ticket_promedio,
       MIN(fecha)        AS primer_pedido,
       MAX(fecha)        AS ultimo_pedido
FROM pedidos
WHERE estado = 'entregado';` },
        { t: "res", head: ["pedidos", "venta_total", "ticket_promedio", "primer_pedido", "ultimo_pedido"],
          rows: [["1,284", "612,430.50", "476.97", "2025-01-03", "2026-08-21"]],
          meta: "1 fila · resumen de 1,284 pedidos" },
        { t: "p", x: "Nota la diferencia entre `COUNT(*)` y `COUNT(telefono)`: la primera cuenta filas, la segunda cuenta valores presentes. Restar una de otra te dice cuántos nulos tienes, y esa resta es un chequeo de calidad gratuito." },
        { t: "ej", x: "Calcula, en una sola consulta, cuántos productos hay, cuántos están activos y cuál es el precio promedio de la tienda." },
        { t: "clave", x: "`COUNT(*)` cuenta filas; `COUNT(columna)` cuenta valores no nulos. La diferencia entre ambos es tu medida de huecos." },
      ],
    },
    {
      id: "m3s2",
      titulo: "GROUP BY: un resumen por categoría",
      objetivo: "Calcular el mismo número para cada grupo.",
      bloques: [
        { t: "p", x: "`GROUP BY` parte la tabla en montoncitos y aplica la agregación a cada uno. La regla de oro: **toda columna del `SELECT` que no esté dentro de una función de agregación tiene que estar en el `GROUP BY`**." },
        { t: "sql", cap: "Venta por ciudad", x: `SELECT ciudad,
       COUNT(*)             AS pedidos,
       SUM(total)           AS venta,
       ROUND(AVG(total), 2) AS ticket
FROM pedidos
WHERE estado = 'entregado'
GROUP BY ciudad
ORDER BY venta DESC;` },
        { t: "res", head: ["ciudad", "pedidos", "venta", "ticket"],
          rows: [["Culiacán", "742", "381,204.00", "513.75"], ["Mazatlán", "356", "156,880.50", "440.68"], ["Los Mochis", "186", "74,346.00", "399.71"]],
          meta: "3 filas · agrupado por ciudad" },
        { t: "p", x: "Puedes agrupar por varias columnas: `GROUP BY ciudad, categoria` te da una fila por cada combinación que exista. El número de filas del resultado es el número de grupos, no el de la tabla original." },
        { t: "nota", et: "Piénsalo así", x: "El `GROUP BY` es la pregunta «¿por cada qué?». Ventas **por ciudad**, clientes **por mes**, ticket **por categoría**. Cuando alguien te pida un reporte, busca el «por» de la frase: ahí está tu `GROUP BY`." },
        { t: "ej", x: "Saca la venta por mes. Pista: necesitas cortar la fecha, por ejemplo con `strftime('%Y-%m', fecha)` en SQLite o `DATE_TRUNC('month', fecha)` en PostgreSQL." },
        { t: "clave", x: "Busca el «por» en la pregunta del negocio. Ese sustantivo es tu `GROUP BY`." },
      ],
    },
    {
      id: "m3s3",
      titulo: "HAVING: filtrar grupos, no filas",
      objetivo: "Distinguir cuándo filtrar antes y cuándo después.",
      bloques: [
        { t: "p", x: "`WHERE` filtra filas **antes** de agrupar. `HAVING` filtra grupos **después** de agrupar. Por eso `HAVING` puede usar `SUM()` o `COUNT()` y `WHERE` no." },
        { t: "sql", cap: "Ciudades con más de 200 pedidos", x: `SELECT ciudad, COUNT(*) AS pedidos, SUM(total) AS venta
FROM pedidos
WHERE estado = 'entregado'      -- filtra pedidos
GROUP BY ciudad
HAVING COUNT(*) > 200           -- filtra ciudades
ORDER BY venta DESC;` },
        { t: "res", head: ["ciudad", "pedidos", "venta"],
          rows: [["Culiacán", "742", "381,204.00"], ["Mazatlán", "356", "156,880.50"]],
          meta: "2 filas · Los Mochis quedó fuera (186)" },
        { t: "nota", et: "Regla práctica", x: "Si la condición habla de una fila (`estado`, `fecha`, `ciudad`) va en `WHERE`. Si habla de un resultado calculado (`COUNT`, `SUM`, `AVG`) va en `HAVING`. Poner todo en `HAVING` funciona pero es más lento: el motor agrupa filas que iba a tirar." },
        { t: "ej", x: "Encuentra los clientes con 3 o más pedidos entregados en los últimos 90 días. Acabas de contestar la pregunta de «clientes leales» del módulo 1." },
        { t: "clave", x: "`WHERE` filtra ingredientes; `HAVING` filtra platillos terminados." },
      ],
    },
    {
      id: "m3s4",
      titulo: "El orden real de ejecución",
      objetivo: "Entender por qué un alias a veces no funciona.",
      bloques: [
        { t: "p", x: "Escribes la consulta en un orden, pero el motor la ejecuta en otro. Memorizar esta secuencia te va a ahorrar cientos de errores confusos." },
        { t: "res", head: ["#", "se ejecuta", "qué hace"],
          rows: [["1", "FROM / JOIN", "arma la tabla de trabajo"], ["2", "WHERE", "tira filas"], ["3", "GROUP BY", "forma los grupos"], ["4", "HAVING", "tira grupos"], ["5", "SELECT", "calcula columnas y alias"], ["6", "ORDER BY", "ordena el resultado"], ["7", "LIMIT", "recorta"]],
          meta: "7 pasos · el orden que importa" },
        { t: "p", x: "Con esa tabla se explica todo. El alias que creas en `SELECT` (paso 5) no existe todavía en `WHERE` (paso 2), por eso falla; pero sí existe en `ORDER BY` (paso 6), por eso ahí sí funciona." },
        { t: "sql", cap: "Lo que se puede y lo que no", x: `SELECT total * 0.16 AS iva
FROM pedidos
WHERE iva > 100          -- ✗ error: 'iva' aún no existe
ORDER BY iva DESC;       -- ✓ correcto: aquí ya existe` },
        { t: "ej", x: "Escribe una consulta que falle por usar un alias en el `WHERE` y arréglala repitiendo la expresión completa. Sentir el error una vez vale más que leerlo diez." },
        { t: "clave", x: "FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT. Apréndetelo de memoria." },
      ],
    },
    {
      id: "m3s5",
      titulo: "CASE WHEN: clasificar dentro de la consulta",
      objetivo: "Crear categorías propias sin salir de SQL.",
      bloques: [
        { t: "p", x: "`CASE WHEN` es el «si… entonces» de SQL. Sirve para etiquetar filas y, combinado con `SUM`, para contar condicionalmente, que es uno de los trucos más rentables del oficio." },
        { t: "sql", cap: "Etiquetar tickets", x: `SELECT id_pedido,
       total,
       CASE
         WHEN total >= 1000 THEN 'grande'
         WHEN total >= 400  THEN 'medio'
         ELSE 'chico'
       END AS tamano
FROM pedidos;` },
        { t: "sql", cap: "Contar condicionalmente (pivote a mano)", x: `SELECT ciudad,
       COUNT(*) AS pedidos,
       SUM(CASE WHEN estado = 'cancelado' THEN 1 ELSE 0 END) AS cancelados,
       ROUND(100.0 * SUM(CASE WHEN estado = 'cancelado' THEN 1 ELSE 0 END)
             / COUNT(*), 1) AS pct_cancelacion
FROM pedidos
GROUP BY ciudad;` },
        { t: "res", head: ["ciudad", "pedidos", "cancelados", "pct_cancelacion"],
          rows: [["Culiacán", "812", "70", "8.6"], ["Mazatlán", "402", "46", "11.4"], ["Los Mochis", "205", "19", "9.3"]],
          meta: "3 filas · un pivote sin salir de SQL" },
        { t: "nota", et: "Detalle numérico", x: "Escribe `100.0` y no `100`: si divides enteros, muchos motores tiran los decimales y te devuelven 0. Es el error silencioso más común en cálculos de porcentaje." },
        { t: "ej", x: "Clasifica a los clientes en «nuevo», «recurrente» y «dormido» según su número de pedidos y la fecha del último. Esa clasificación es, literalmente, un análisis de retención." },
        { t: "clave", x: "`SUM(CASE WHEN … THEN 1 ELSE 0 END)` convierte cualquier condición en una columna de conteo. Es tu pivote portátil." },
      ],
    },
  ],
};

const M4 = {
  id: "m4", n: "04",
  titulo: "Combinar tablas",
  resumen: "Los JOIN: donde SQL deja de ser una lista y se vuelve un modelo.",
  sesiones: [
    {
      id: "m4s1",
      titulo: "Claves primarias y foráneas",
      objetivo: "Saber por qué columna se pegan dos tablas.",
      bloques: [
        { t: "p", x: "La **clave primaria** es la columna que identifica una fila de forma única: `id_cliente` en `clientes`. La **clave foránea** es esa misma columna viviendo en otra tabla: `id_cliente` dentro de `pedidos`. Ese par es la costura." },
        { t: "p", x: "La relación casi siempre es **uno a muchos**: un cliente tiene muchos pedidos, un pedido tiene muchos renglones de detalle. Saber de qué lado está el «muchos» te dice qué va a pasar con el número de filas al unir." },
        { t: "res", head: ["desde", "hacia", "relación", "columna puente"],
          rows: [["clientes", "pedidos", "1 → muchos", "id_cliente"], ["pedidos", "detalle", "1 → muchos", "id_pedido"], ["productos", "detalle", "1 → muchos", "id_producto"], ["empleados", "empleados", "1 → muchos", "id_jefe"]],
          meta: "4 relaciones de La Higuera" },
        { t: "nota", et: "Señal de alarma", x: "Si al unir dos tablas te salen más filas de las que tenía la tabla grande, no está mal el `JOIN`: es que la columna puente **no era única** del otro lado. Casi siempre significa que hay duplicados escondidos." },
        { t: "ej", x: "Antes de unir, corre `SELECT id_cliente, COUNT(*) FROM clientes GROUP BY id_cliente HAVING COUNT(*) > 1`. Si devuelve filas, tu clave primaria no lo es tanto." },
        { t: "clave", x: "Antes de escribir un `JOIN`, di en voz alta: «un X tiene muchos Y». Si no puedes decirlo, todavía no entiendes el modelo." },
      ],
    },
    {
      id: "m4s2",
      titulo: "INNER JOIN: lo que está en ambas",
      objetivo: "Unir dos tablas por su columna común.",
      bloques: [
        { t: "p", x: "`INNER JOIN` conserva solo las filas que encuentran pareja en las dos tablas. Es el join por omisión y el que más vas a usar." },
        { t: "sql", cap: "Pedidos con nombre de cliente", x: `SELECT p.id_pedido,
       c.nombre,
       c.ciudad,
       p.fecha,
       p.total
FROM pedidos AS p
INNER JOIN clientes AS c ON c.id_cliente = p.id_cliente
WHERE p.estado = 'entregado'
ORDER BY p.fecha DESC;` },
        { t: "res", head: ["id_pedido", "nombre", "ciudad", "fecha", "total"],
          rows: [["1092", "Cynthia Ibarra", "Culiacán", "2026-08-21", "612.00"], ["1091", "Ana Rocha", "Culiacán", "2026-08-21", "348.50"], ["1088", "Diego Payán", "Mazatlán", "2026-08-20", "1,730.00"]],
          meta: "3 de 1,284 filas" },
        { t: "p", x: "Los alias de tabla (`p`, `c`) no son adorno: cuando unes cuatro tablas y todas tienen una columna `nombre`, el prefijo es lo único que evita la ambigüedad. Prefija **siempre** todas las columnas en una consulta con joins." },
        { t: "h", x: "Unir tres o más tablas" },
        { t: "sql", cap: "Ticket completo", x: `SELECT p.id_pedido, c.nombre, pr.nombre AS producto,
       d.cantidad, d.precio_unit,
       d.cantidad * d.precio_unit AS importe
FROM pedidos     AS p
JOIN clientes    AS c  ON c.id_cliente  = p.id_cliente
JOIN detalle     AS d  ON d.id_pedido   = p.id_pedido
JOIN productos   AS pr ON pr.id_producto = d.id_producto
WHERE p.id_pedido = 1042;` },
        { t: "ej", x: "Calcula la venta total por categoría de producto. Necesitas `detalle` + `productos`, `SUM(cantidad * precio_unit)` y un `GROUP BY`. Es tu primera consulta de verdad." },
        { t: "clave", x: "Un `JOIN` es una condición de igualdad entre dos columnas. Todo lo demás es alias y disciplina." },
      ],
    },
    {
      id: "m4s3",
      titulo: "LEFT JOIN y lo que revela el hueco",
      objetivo: "Conservar filas sin pareja y usar los nulos como información.",
      bloques: [
        { t: "p", x: "`LEFT JOIN` conserva **todas** las filas de la tabla izquierda, tengan pareja o no. Donde no hay pareja, las columnas de la derecha vienen en `NULL`. Ese hueco suele ser el hallazgo." },
        { t: "sql", cap: "Todos los clientes, hayan comprado o no", x: `SELECT c.nombre,
       COUNT(p.id_pedido) AS pedidos,
       COALESCE(SUM(p.total), 0) AS gastado
FROM clientes AS c
LEFT JOIN pedidos AS p ON p.id_cliente = c.id_cliente
GROUP BY c.nombre
ORDER BY gastado DESC;` },
        { t: "res", head: ["nombre", "pedidos", "gastado"],
          rows: [["Diego Payán", "31", "18,904.00"], ["Ana Rocha", "12", "5,240.50"], ["Elena Zazueta", "0", "0.00"]],
          meta: "3 filas · Elena nunca ha comprado" },
        { t: "h", x: "El patrón antijoin" },
        { t: "p", x: "Si lo que quieres son justo los que **no** tienen pareja, filtra por el nulo. Es la forma estándar de encontrar clientes sin pedidos, productos sin ventas o pedidos huérfanos." },
        { t: "sql", cap: "Clientes que nunca compraron", x: `SELECT c.id_cliente, c.nombre
FROM clientes AS c
LEFT JOIN pedidos AS p ON p.id_cliente = c.id_cliente
WHERE p.id_pedido IS NULL;` },
        { t: "nota", et: "Trampa muy común", x: "Si pones la condición de la tabla derecha en el `WHERE` (`WHERE p.estado = 'entregado'`), tu `LEFT JOIN` se comporta como `INNER JOIN`, porque el nulo no pasa el filtro. Esa condición va **dentro del `ON`**." },
        { t: "ej", x: "Lista los productos activos que no se han vendido nunca. Es un reporte que cualquier tienda quiere y son seis líneas de SQL." },
        { t: "clave", x: "Condición sobre la tabla izquierda → `WHERE`. Condición sobre la derecha en un `LEFT JOIN` → `ON`." },
      ],
    },
    {
      id: "m4s4",
      titulo: "Los demás joins y el self join",
      objetivo: "Reconocer RIGHT, FULL, CROSS y unir una tabla consigo misma.",
      bloques: [
        { t: "res", head: ["join", "conserva", "cuándo se usa"],
          rows: [["INNER", "solo coincidencias", "el 80% de los casos"], ["LEFT", "todo lo de la izquierda", "detectar ausencias"], ["RIGHT", "todo lo de la derecha", "casi nunca: dale la vuelta y usa LEFT"], ["FULL", "todo de ambos lados", "conciliar dos fuentes"], ["CROSS", "todas las combinaciones", "generar calendarios o rejillas"]],
          meta: "5 tipos de unión" },
        { t: "p", x: "El **self join** une una tabla consigo misma usando dos alias distintos. Sirve para jerarquías (empleado y su jefe) y para comparar filas de la misma tabla entre sí." },
        { t: "sql", cap: "Cada empleado con su jefe", x: `SELECT e.nombre        AS empleado,
       j.nombre        AS jefe
FROM empleados AS e
LEFT JOIN empleados AS j ON j.id_empleado = e.id_jefe;` },
        { t: "res", head: ["empleado", "jefe"],
          rows: [["Rosa Camacho", "—"], ["Iván Quiñónez", "Rosa Camacho"], ["Lupita Verdugo", "Rosa Camacho"], ["Toño Bátiz", "Iván Quiñónez"]],
          meta: "4 filas · Rosa es la dueña" },
        { t: "p", x: "El `CROSS JOIN` multiplica: 30 días × 4 sucursales = 120 filas. Suena inútil hasta que necesitas un reporte con **todos** los días, incluidos los que no vendiste nada; entonces cruzas un calendario contra tus sucursales y le pegas las ventas con `LEFT JOIN`." },
        { t: "ej", x: "Arma la rejilla de los últimos 7 días × 3 ciudades y pégale las ventas reales. Los ceros que aparezcan son días muertos que tu reporte anterior escondía." },
        { t: "clave", x: "Los días sin venta no aparecen solos: hay que fabricarlos con un cruce y descubrirlos con un `LEFT JOIN`." },
      ],
    },
    {
      id: "m4s5",
      titulo: "UNION: apilar resultados",
      objetivo: "Juntar dos consultas una debajo de la otra.",
      bloques: [
        { t: "p", x: "Un `JOIN` pega tablas **a lo ancho**; un `UNION` las apila **a lo alto**. Las dos consultas deben tener el mismo número de columnas y tipos compatibles." },
        { t: "sql", cap: "Historia y archivo en un solo reporte", x: `SELECT id_pedido, fecha, total FROM pedidos
UNION ALL
SELECT id_pedido, fecha, total FROM pedidos_2024
ORDER BY fecha DESC;` },
        { t: "ul", x: [
          "`UNION` elimina filas duplicadas (y por eso es más lento).",
          "`UNION ALL` apila todo tal cual: úsalo por omisión si sabes que no hay repetidos.",
          "`INTERSECT` deja solo lo que está en ambas consultas.",
          "`EXCEPT` (o `MINUS`) deja lo de la primera que no está en la segunda.",
        ] },
        { t: "nota", et: "Uso real", x: "`EXCEPT` es la mejor herramienta para conciliar: corre el reporte viejo y el nuevo, réstalos, y las filas que salgan son exactamente tus diferencias. Vale más que revisar dos hojas de Excel en paralelo." },
        { t: "ej", x: "Con `EXCEPT`, encuentra qué `id_cliente` aparecen en `pedidos` pero ya no existen en `clientes`. Esos son registros huérfanos y son un problema de integridad." },
        { t: "clave", x: "JOIN = a lo ancho. UNION = a lo alto. EXCEPT = tu detector de diferencias." },
      ],
    },
  ],
};

const M5 = {
  id: "m5", n: "05",
  titulo: "SQL avanzado",
  resumen: "Subconsultas, CTE, funciones de ventana, fechas, texto y rendimiento.",
  sesiones: [
    {
      id: "m5s1",
      titulo: "Subconsultas: una consulta dentro de otra",
      objetivo: "Usar el resultado de una consulta como insumo de otra.",
      bloques: [
        { t: "p", x: "Una subconsulta es una consulta entre paréntesis. Puede devolver un valor, una lista o una tabla, y según eso se coloca en un lugar distinto." },
        { t: "sql", cap: "Un valor: pedidos arriba del promedio", x: `SELECT id_pedido, total
FROM pedidos
WHERE total > (SELECT AVG(total) FROM pedidos);` },
        { t: "sql", cap: "Una lista: clientes de ciudades grandes", x: `SELECT nombre
FROM clientes
WHERE ciudad IN (
  SELECT ciudad FROM pedidos
  GROUP BY ciudad HAVING COUNT(*) > 200
);` },
        { t: "sql", cap: "Una tabla: se usa como si fuera una tabla", x: `SELECT ciudad, ROUND(AVG(venta), 2) AS venta_promedio_mensual
FROM (
  SELECT ciudad, strftime('%Y-%m', fecha) AS mes, SUM(total) AS venta
  FROM pedidos
  GROUP BY ciudad, mes
) AS resumen
GROUP BY ciudad;` },
        { t: "p", x: "Esa última se llama **subconsulta derivada** y resuelve un problema muy real: agregar dos veces (primero por mes, luego el promedio de esos meses). No se puede hacer en un solo `GROUP BY`." },
        { t: "nota", et: "Rendimiento", x: "Una subconsulta **correlacionada** (la que menciona la tabla de afuera) se ejecuta una vez por fila. Con 10 filas ni lo notas; con 10 millones tumbas el servidor. Casi siempre se puede reescribir como `JOIN`." },
        { t: "ej", x: "Trae los clientes cuyo gasto total supera el gasto promedio de todos los clientes. Vas a necesitar agregación dentro de agregación." },
        { t: "clave", x: "Si tu subconsulta pasa de tres líneas, deja de anidar y pásate a un CTE: es la siguiente sesión." },
      ],
    },
    {
      id: "m5s2",
      titulo: "CTE: escribir SQL que se pueda leer",
      objetivo: "Partir una consulta grande en pasos con nombre.",
      bloques: [
        { t: "p", x: "`WITH` crea una tabla temporal con nombre que solo existe durante la consulta. Se llama CTE (expresión de tabla común) y es el mayor salto de calidad que puedes darle a tu SQL." },
        { t: "sql", cap: "La misma lógica, legible", x: `WITH venta_mensual AS (
  SELECT ciudad,
         strftime('%Y-%m', fecha) AS mes,
         SUM(total) AS venta
  FROM pedidos
  WHERE estado = 'entregado'
  GROUP BY ciudad, mes
),
promedio_ciudad AS (
  SELECT ciudad, ROUND(AVG(venta), 2) AS promedio
  FROM venta_mensual
  GROUP BY ciudad
)
SELECT v.ciudad, v.mes, v.venta, p.promedio,
       ROUND(v.venta - p.promedio, 2) AS diferencia
FROM venta_mensual AS v
JOIN promedio_ciudad AS p ON p.ciudad = v.ciudad
ORDER BY v.ciudad, v.mes;` },
        { t: "res", head: ["ciudad", "mes", "venta", "promedio", "diferencia"],
          rows: [["Culiacán", "2026-06", "34,120.00", "31,767.00", "2,353.00"], ["Culiacán", "2026-07", "29,880.50", "31,767.00", "-1,886.50"], ["Mazatlán", "2026-06", "14,205.00", "13,073.38", "1,131.62"]],
          meta: "3 filas · dos pasos encadenados" },
        { t: "ul", x: [
          "Cada CTE es un paso con nombre: se lee de arriba hacia abajo, como una receta.",
          "Un CTE puede usar al anterior, y así encadenas transformaciones.",
          "Se depura por partes: comentas el `SELECT` final y consultas un CTE suelto.",
        ] },
        { t: "ej", x: "Reescribe con CTE la consulta más enredada que hayas hecho hasta ahora. Ponle a cada paso un nombre que explique qué hace, no cómo lo hace: `clientes_activos`, no `paso2`." },
        { t: "clave", x: "El SQL profesional no es el más ingenioso: es el que otra persona entiende en un minuto." },
      ],
    },
    {
      id: "m5s3",
      titulo: "Funciones de ventana I: numerar y rankear",
      objetivo: "Calcular por grupo sin perder el detalle de las filas.",
      bloques: [
        { t: "p", x: "`GROUP BY` colapsa las filas; una **función de ventana** calcula sobre un grupo pero **deja las filas intactas**. Esa es toda la diferencia, y abre la mitad de las preguntas interesantes." },
        { t: "sql", cap: "Ranking dentro de cada categoría", x: `SELECT categoria, nombre, precio,
       ROW_NUMBER() OVER (PARTITION BY categoria ORDER BY precio DESC) AS num,
       RANK()       OVER (PARTITION BY categoria ORDER BY precio DESC) AS rango
FROM productos
WHERE activo = 1;` },
        { t: "res", head: ["categoria", "nombre", "precio", "num", "rango"],
          rows: [["Bebidas", "Café molido 500 g", "128.00", "1", "1"], ["Bebidas", "Refresco 2 L", "38.00", "2", "2"], ["Abarrotes", "Aceite 1 L", "112.00", "1", "1"], ["Abarrotes", "Harina 1 kg", "32.50", "2", "2"]],
          meta: "4 filas · el detalle se conserva" },
        { t: "ul", x: [
          "`PARTITION BY` define el grupo (equivale al `GROUP BY` de la ventana).",
          "`ORDER BY` dentro del `OVER` define el orden **dentro** de cada grupo.",
          "`ROW_NUMBER` nunca empata; `RANK` deja huecos tras un empate; `DENSE_RANK` no los deja.",
        ] },
        { t: "h", x: "El top N por grupo, por fin" },
        { t: "sql", cap: "Los 3 productos más caros por categoría", x: `WITH ordenados AS (
  SELECT categoria, nombre, precio,
         ROW_NUMBER() OVER (PARTITION BY categoria ORDER BY precio DESC) AS n
  FROM productos
)
SELECT categoria, nombre, precio
FROM ordenados
WHERE n <= 3;` },
        { t: "nota", et: "Por qué el CTE", x: "No puedes filtrar una función de ventana en el `WHERE`: la ventana se calcula en el paso 5 (el `SELECT`) y el `WHERE` es el paso 2. Por eso se envuelve en un CTE y se filtra afuera." },
        { t: "ej", x: "Saca el pedido más caro de cada cliente, con fecha y monto. Es la pregunta de ventana más pedida en entrevistas." },
        { t: "clave", x: "Ventana = calcular por grupo sin perder las filas. Para filtrar el resultado, envuélvelo en un CTE." },
      ],
    },
    {
      id: "m5s4",
      titulo: "Funciones de ventana II: comparar con la fila anterior",
      objetivo: "Medir crecimiento, acumulados y medias móviles.",
      bloques: [
        { t: "p", x: "`LAG` mira la fila anterior y `LEAD` la siguiente. Con eso calculas variación mes contra mes sin salir de SQL, que es probablemente la métrica más pedida en cualquier empresa." },
        { t: "sql", cap: "Crecimiento mes contra mes", x: `WITH mensual AS (
  SELECT strftime('%Y-%m', fecha) AS mes, SUM(total) AS venta
  FROM pedidos WHERE estado = 'entregado'
  GROUP BY mes
)
SELECT mes, venta,
       LAG(venta) OVER (ORDER BY mes) AS mes_anterior,
       ROUND(100.0 * (venta - LAG(venta) OVER (ORDER BY mes))
             / LAG(venta) OVER (ORDER BY mes), 1) AS var_pct
FROM mensual;` },
        { t: "res", head: ["mes", "venta", "mes_anterior", "var_pct"],
          rows: [["2026-05", "48,320.00", "—", "—"], ["2026-06", "52,905.00", "48,320.00", "9.5"], ["2026-07", "44,180.50", "52,905.00", "-16.5"], ["2026-08", "51,660.00", "44,180.50", "16.9"]],
          meta: "4 filas · la primera no tiene anterior" },
        { t: "h", x: "Acumulados y medias móviles" },
        { t: "sql", cap: "Marco de ventana", x: `SELECT mes, venta,
       SUM(venta) OVER (ORDER BY mes
            ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS acumulado,
       ROUND(AVG(venta) OVER (ORDER BY mes
            ROWS BETWEEN 2 PRECEDING AND CURRENT ROW), 2) AS media_3m
FROM mensual;` },
        { t: "p", x: "El `ROWS BETWEEN` define cuántas filas entran en el cálculo: desde el inicio hasta la actual da un acumulado; las dos anteriores más la actual dan una media móvil de tres periodos, que suaviza el ruido de un mes raro." },
        { t: "ej", x: "Calcula el acumulado de venta del año en curso y la media móvil de 3 meses. Grafícalo mentalmente: así se ve un tablero de dirección." },
        { t: "clave", x: "`LAG` para comparar, marcos de ventana para acumular y suavizar. Con esto ya haces reportes de dirección." },
      ],
    },
    {
      id: "m5s5",
      titulo: "Fechas y tiempo",
      objetivo: "Cortar, agrupar y comparar periodos sin sufrir.",
      bloques: [
        { t: "p", x: "Las fechas son la dimensión más usada y la que más varía entre motores. Aprende el concepto y busca la sintaxis exacta de tu motor: se ve distinto, hace lo mismo." },
        { t: "res", head: ["quiero", "PostgreSQL", "SQLite", "MySQL"],
          rows: [["hoy", "CURRENT_DATE", "DATE('now')", "CURDATE()"], ["cortar a mes", "DATE_TRUNC('month', f)", "strftime('%Y-%m', f)", "DATE_FORMAT(f,'%Y-%m')"], ["restar 30 días", "f - INTERVAL '30 days'", "DATE(f,'-30 day')", "f - INTERVAL 30 DAY"], ["diferencia en días", "f2 - f1", "JULIANDAY(f2)-JULIANDAY(f1)", "DATEDIFF(f2,f1)"]],
          meta: "misma idea, tres dialectos" },
        { t: "sql", cap: "Últimos 30 días y cohorte de alta", x: `SELECT c.nombre,
       c.fecha_alta,
       COUNT(p.id_pedido) AS pedidos_30d
FROM clientes AS c
LEFT JOIN pedidos AS p
       ON p.id_cliente = c.id_cliente
      AND p.fecha >= DATE('now', '-30 day')
GROUP BY c.nombre, c.fecha_alta;` },
        { t: "ul", x: [
          "Guarda siempre las fechas como tipo fecha, nunca como texto libre: `'03/08/26'` es imposible de ordenar y ambiguo entre países.",
          "Cuidado con la zona horaria: un pedido de las 11 de la noche puede caer en otro día según el huso con el que consultes.",
          "Para comparar periodos, calcula ambos en un CTE y únelos; no lo hagas a mano.",
        ] },
        { t: "ej", x: "Compara la venta de este mes contra el mismo mes del año pasado, en una sola tabla con las dos columnas y la variación." },
        { t: "clave", x: "Fecha como texto es una bomba de tiempo. Tipo fecha, formato AAAA-MM-DD, zona horaria decidida." },
      ],
    },
    {
      id: "m5s6",
      titulo: "Texto: limpiar sin salir de SQL",
      objetivo: "Normalizar nombres, correos y categorías desordenadas.",
      bloques: [
        { t: "p", x: "El texto que capturan las personas siempre viene sucio: espacios de más, mayúsculas inconsistentes, acentos a medias. Estas funciones resuelven el 90%." },
        { t: "ul", x: [
          "`TRIM(texto)` quita espacios al inicio y al final; `LTRIM`/`RTRIM` solo de un lado.",
          "`LOWER` / `UPPER` para comparar sin depender de mayúsculas.",
          "`REPLACE(texto, 'viejo', 'nuevo')` sustituye subcadenas.",
          "`SUBSTR(texto, inicio, largo)` recorta; `LENGTH` mide.",
          "`||` (o `CONCAT`) pega textos: `nombre || ' ' || apellido`.",
        ] },
        { t: "sql", cap: "Ciudades normalizadas", x: `SELECT TRIM(LOWER(ciudad)) AS ciudad_limpia,
       COUNT(*) AS clientes
FROM clientes
GROUP BY ciudad_limpia
ORDER BY clientes DESC;` },
        { t: "res", head: ["ciudad_limpia", "clientes"],
          rows: [["culiacán", "418"], ["mazatlán", "203"], ["los mochis", "97"]],
          meta: "3 filas · antes eran 7 variantes" },
        { t: "nota", et: "Regla de oro", x: "Limpiar en la consulta está bien para explorar. Si el mismo `TRIM(LOWER(...))` aparece en cinco reportes, el arreglo va en el origen o en una vista, no repetido en cada consulta." },
        { t: "ej", x: "Extrae el dominio de los correos (`SUBSTR` desde la posición del `@`) y cuenta cuántos clientes usan cada uno. Es un dato de segmentación gratis." },
        { t: "clave", x: "Se limpia una vez y en el lugar correcto. Copiar la limpieza en cada consulta es deuda que se paga con reportes que no cuadran." },
      ],
    },
    {
      id: "m5s7",
      titulo: "Rendimiento: índices y plan de ejecución",
      objetivo: "Entender por qué una consulta tarda y qué hacer.",
      bloques: [
        { t: "p", x: "Un **índice** es una estructura ordenada aparte que le permite al motor saltar directo a las filas que busca, en lugar de leer la tabla completa. Es el índice de un libro: sin él, hojeas todo." },
        { t: "sql", cap: "Crear un índice y pedir el plan", x: `CREATE INDEX idx_pedidos_cliente_fecha
  ON pedidos (id_cliente, fecha);

EXPLAIN QUERY PLAN            -- SQLite
SELECT * FROM pedidos WHERE id_cliente = 7;` },
        { t: "ul", x: [
          "Indexa las columnas por las que **filtras** y **unes**: claves foráneas y fechas, casi siempre.",
          "Cada índice acelera lecturas y frena escrituras: no indexes todo.",
          "Una función sobre la columna anula el índice: `WHERE YEAR(fecha) = 2026` no lo usa; `WHERE fecha >= '2026-01-01'` sí.",
          "`SELECT *` sobre tablas anchas trae megas que no vas a mirar.",
        ] },
        { t: "p", x: "Busca en el plan las palabras `SCAN` (leyó toda la tabla) frente a `SEARCH` o `INDEX` (usó un índice). En PostgreSQL sería `Seq Scan` frente a `Index Scan`. Esa palabra te dice si tu consulta escala o no." },
        { t: "nota", et: "En la práctica", x: "Antes de optimizar, mide. Una consulta que tarda 2 segundos y corre una vez al mes no es un problema; una de 200 ms que corre en un tablero cada 5 segundos sí lo es." },
        { t: "ej", x: "Corre una consulta pesada, mira el plan, crea el índice adecuado y vuelve a mirarlo. Ver cambiar `SCAN` por `SEARCH` es el momento en que el rendimiento deja de ser magia." },
        { t: "clave", x: "Índices en lo que filtras y unes. Nunca envuelvas en función la columna indexada." },
      ],
    },
  ],
};

const M6 = {
  id: "m6", n: "06",
  titulo: "Modelar y cuidar los datos",
  resumen: "Cómo se diseña, se carga y se ensucia una base — y cómo se arregla.",
  sesiones: [
    {
      id: "m6s1",
      titulo: "Normalización: por qué las tablas están partidas",
      objetivo: "Entender el diseño relacional que vas a consultar toda tu vida.",
      bloques: [
        { t: "p", x: "Normalizar es guardar cada dato **una sola vez**. Si el nombre del cliente vive en la tabla `clientes` y en ningún otro lado, corregir una falta de ortografía es cambiar una fila. Si estuviera copiado en cada pedido, tendrías que cambiar miles y algunas se te iban a escapar." },
        { t: "res", head: ["forma", "regla", "en cristiano"],
          rows: [["1FN", "un valor por celda", "nada de «tel1, tel2» en una columna"], ["2FN", "todo depende de la clave completa", "no mezcles datos de dos cosas"], ["3FN", "nada depende de otra columna no clave", "la ciudad del cliente no va en el pedido"]],
          meta: "3 formas normales · suficiente para el 99%" },
        { t: "p", x: "El costo de normalizar es que necesitas `JOIN` para todo. El beneficio es que no hay contradicciones. Las bases de operación (donde se registra la venta) se normalizan; los almacenes de análisis, no tanto, y eso lo vemos en la sesión siguiente." },
        { t: "ej", x: "Toma una hoja de cálculo real que uses y pártela en tablas normalizadas. Vas a descubrir que la hoja mezclaba tres cosas distintas: eso explica por qué era imposible mantenerla." },
        { t: "clave", x: "Un dato, un lugar. Todo lo demás son referencias a ese lugar." },
      ],
    },
    {
      id: "m6s2",
      titulo: "Modelo estrella: cómo se ordenan los datos para analizar",
      objetivo: "Distinguir tablas de hechos y de dimensiones.",
      bloques: [
        { t: "p", x: "Cuando el objetivo es analizar y no registrar, el diseño cambia. Se arma un **modelo estrella**: una tabla de **hechos** en el centro (los eventos medibles) rodeada de tablas de **dimensiones** (el contexto con el que los cortas)." },
        { t: "res", head: ["tipo", "contiene", "ejemplo en La Higuera"],
          rows: [["Hechos", "métricas + claves", "ventas: cantidad, importe, id_fecha, id_producto"], ["Dimensión", "atributos descriptivos", "dim_producto: nombre, categoría, marca"], ["Dimensión", "el calendario, siempre", "dim_fecha: día, mes, trimestre, festivo"]],
          meta: "1 hecho + N dimensiones = estrella" },
        { t: "p", x: "La tabla de hechos es larga y flaca (millones de filas, pocas columnas); las dimensiones son cortas y anchas. Con ese diseño, cualquier pregunta se contesta igual: agrega una métrica del hecho y córtala por atributos de las dimensiones." },
        { t: "nota", et: "Dimensión fecha", x: "Casi todo modelo serio tiene una tabla de calendario con una fila por día y columnas para mes, trimestre, día de la semana y festivos. Suena exagerado hasta que te piden «ventas en fin de semana contra entre semana» y son dos líneas." },
        { t: "ej", x: "Diseña en papel el modelo estrella de La Higuera: una tabla de hechos de ventas y las dimensiones cliente, producto, fecha y sucursal." },
        { t: "clave", x: "Hechos = lo que mides. Dimensiones = las formas de cortarlo. Toda pregunta de negocio es una métrica cortada por dimensiones." },
      ],
    },
    {
      id: "m6s3",
      titulo: "Calidad de datos: duplicados, huecos y rarezas",
      objetivo: "Auditar una tabla antes de confiar en ella.",
      bloques: [
        { t: "p", x: "Nunca entregues un número sin auditar la tabla. Estas cuatro consultas se corren siempre y en este orden: son diez minutos que te evitan una junta incómoda." },
        { t: "sql", cap: "1 · ¿Hay duplicados?", x: `SELECT id_pedido, COUNT(*) AS veces
FROM pedidos
GROUP BY id_pedido
HAVING COUNT(*) > 1;` },
        { t: "sql", cap: "2 · ¿Cuántos huecos hay por columna?", x: `SELECT COUNT(*) AS filas,
       COUNT(*) - COUNT(ciudad)  AS sin_ciudad,
       COUNT(*) - COUNT(total)   AS sin_total,
       COUNT(*) - COUNT(fecha)   AS sin_fecha
FROM pedidos;` },
        { t: "sql", cap: "3 · ¿Los valores tienen sentido?", x: `SELECT MIN(total) AS minimo, MAX(total) AS maximo,
       MIN(fecha) AS desde,  MAX(fecha) AS hasta
FROM pedidos;` },
        { t: "sql", cap: "4 · ¿Qué categorías existen de verdad?", x: `SELECT estado, COUNT(*) FROM pedidos GROUP BY estado;` },
        { t: "p", x: "Un total negativo, una fecha en 1970 o en 2087, un `estado` escrito de cuatro maneras: todo eso aparece en estas cuatro consultas y todo eso rompe reportes." },
        { t: "nota", et: "Qué hacer con lo raro", x: "No borres sin preguntar. Un valor extremo puede ser un error de captura **o** el pedido más grande del año. Investiga la fila, no la estadística." },
        { t: "ej", x: "Corre las cuatro auditorías sobre cualquier tabla que tengas a la mano y escribe tres frases con lo que encontraste. Ese párrafo es lo que un analista sénior manda antes de entregar." },
        { t: "clave", x: "Duplicados, nulos, rangos y categorías. Cuatro consultas, siempre, antes de cualquier número." },
      ],
    },
    {
      id: "m6s4",
      titulo: "ETL y ELT: cómo llegan los datos a tu base",
      objetivo: "Saber de dónde salen las tablas que consultas.",
      bloques: [
        { t: "p", x: "Los datos no aparecen solos. Alguien los **extrae** de un origen (el punto de venta, una API, un CSV), los **transforma** (limpia, cruza, calcula) y los **carga** en el lugar donde tú consultas." },
        { t: "ul", x: [
          "**ETL**: se transforma antes de cargar. Clásico, útil cuando el destino es caro o rígido.",
          "**ELT**: se carga crudo y se transforma dentro del almacén con SQL. Es lo dominante hoy con BigQuery, Snowflake o Redshift.",
          "**Orquestador**: el que decide cuándo corre cada paso y avisa si falla (Airflow, Dagster, o un cron modesto).",
          "**Capa de transformación**: dbt es el estándar; en el fondo son consultas SQL versionadas en Git.",
        ] },
        { t: "p", x: "Para un analista, la parte que importa es la **frescura**: si el tablero dice «actualizado hoy» pero la carga corre a las 3 a.m., los datos de la mañana no están. La mitad de los reportes «equivocados» son en realidad reportes desfasados." },
        { t: "nota", et: "Pregunta que te va a salvar", x: "Antes de explicar por qué un número bajó, pregunta: ¿a qué hora se cargó esta tabla y qué periodo cubre? Muchas veces la caída es una carga incompleta." },
        { t: "ej", x: "Documenta el linaje de un reporte que uses: de dónde sale cada tabla, cada cuándo se actualiza y quién la mantiene." },
        { t: "clave", x: "Antes de dudar del negocio, duda de la frescura del dato." },
      ],
    },
    {
      id: "m6s5",
      titulo: "Crear y modificar datos (sin borrar producción)",
      objetivo: "Usar DDL y DML con red de seguridad.",
      bloques: [
        { t: "p", x: "Hasta aquí solo has leído. `CREATE`, `INSERT`, `UPDATE` y `DELETE` escriben, y ahí los errores no se deshacen con Ctrl+Z." },
        { t: "sql", cap: "Estructura (DDL) y datos (DML)", x: `CREATE TABLE promociones (
  id_promo   INTEGER PRIMARY KEY,
  nombre     TEXT NOT NULL,
  descuento  REAL CHECK (descuento BETWEEN 0 AND 1),
  vigente_a  DATE
);

INSERT INTO promociones (nombre, descuento, vigente_a)
VALUES ('Martes de abarrotes', 0.15, '2026-12-31');

UPDATE promociones SET descuento = 0.20 WHERE id_promo = 1;
DELETE FROM promociones WHERE vigente_a < DATE('now');` },
        { t: "h", x: "El ritual antes de escribir" },
        { t: "ul", x: [
          "Escribe primero el `SELECT` con el mismo `WHERE` y mira exactamente qué filas vas a tocar.",
          "Abre una transacción: `BEGIN;` … revisa … `COMMIT;` o `ROLLBACK;`.",
          "Nunca corras un `UPDATE` o `DELETE` sin `WHERE`. Nunca.",
          "Respalda antes de tocar producción, aunque «solo sea una fila».",
        ] },
        { t: "nota", et: "Restricciones que te cuidan", x: "`NOT NULL`, `UNIQUE`, `CHECK` y `FOREIGN KEY` son las reglas que la base hace cumplir por ti. Una base con buenas restricciones ensucia menos que cualquier documento de buenas intenciones." },
        { t: "ej", x: "Crea una tabla, mete tres filas, actualiza una dentro de una transacción y haz `ROLLBACK`. Comprueba que no cambió nada: ese es tu paracaídas." },
        { t: "clave", x: "`SELECT` primero, transacción después, `WHERE` siempre." },
      ],
    },
  ],
};

const M7 = {
  id: "m7", n: "07",
  titulo: "Estadística para decidir",
  resumen: "Lo mínimo indispensable para que tus números no engañen a nadie.",
  sesiones: [
    {
      id: "m7s1",
      titulo: "Describir: promedio, mediana y dispersión",
      objetivo: "Elegir la medida que no miente.",
      bloques: [
        { t: "p", x: "El promedio es la medida más usada y la más fácil de romper. Un solo pedido de 50,000 pesos entre cien pedidos de 300 sube el promedio y no describe a nadie." },
        { t: "res", head: ["medida", "qué es", "cuándo usarla"],
          rows: [["media", "suma / cantidad", "datos simétricos, sin extremos"], ["mediana", "el valor de en medio", "ingresos, tickets, tiempos"], ["moda", "el más frecuente", "categorías"], ["desv. estándar", "qué tan disperso está", "acompaña siempre a la media"], ["percentil 90", "el 90% queda debajo", "tiempos de entrega, SLA"]],
          meta: "5 medidas descriptivas" },
        { t: "sql", cap: "Media y mediana en la misma consulta", x: `WITH ordenado AS (
  SELECT total,
         ROW_NUMBER() OVER (ORDER BY total) AS pos,
         COUNT(*)     OVER ()               AS n
  FROM pedidos WHERE estado = 'entregado'
)
SELECT ROUND(AVG(total), 2) AS media,
       (SELECT ROUND(AVG(total), 2) FROM ordenado
        WHERE pos IN ((n+1)/2, (n+2)/2)) AS mediana
FROM ordenado;` },
        { t: "res", head: ["media", "mediana"], rows: [["476.97", "342.00"]], meta: "1 fila · la brecha delata los extremos" },
        { t: "p", x: "Cuando media y mediana se separan mucho, hay cola larga: unos pocos valores muy grandes. Reportar solo la media en ese caso es, técnicamente, correcto y, prácticamente, engañoso." },
        { t: "ej", x: "Calcula media y mediana del ticket por ciudad. Donde más se separen, busca el pedido que lo causa." },
        { t: "clave", x: "Reporta la mediana cuando haya extremos, y siempre acompaña la media con una medida de dispersión." },
      ],
    },
    {
      id: "m7s2",
      titulo: "Distribuciones y percentiles",
      objetivo: "Ver la forma de los datos antes de resumirlos.",
      bloques: [
        { t: "p", x: "Antes de calcular una sola métrica, mira la **forma**: un histograma de diez barras te dice más que cualquier promedio. Verás si los datos se agrupan, si hay dos picos (dos poblaciones mezcladas) o una cola larga." },
        { t: "sql", cap: "Histograma casero", x: `SELECT CASE
         WHEN total < 200  THEN '1 · menos de 200'
         WHEN total < 500  THEN '2 · 200-500'
         WHEN total < 1000 THEN '3 · 500-1000'
         ELSE                   '4 · más de 1000'
       END AS rango,
       COUNT(*) AS pedidos
FROM pedidos
GROUP BY rango ORDER BY rango;` },
        { t: "res", head: ["rango", "pedidos"],
          rows: [["1 · menos de 200", "402"], ["2 · 200-500", "561"], ["3 · 500-1000", "236"], ["4 · más de 1000", "85"]],
          meta: "4 rangos · cola larga a la derecha" },
        { t: "ul", x: [
          "**Percentil**: el p90 de entrega es el tiempo por debajo del cual quedan 9 de cada 10 pedidos.",
          "**Dos picos** casi siempre significan dos grupos distintos: sepáralos y analiza por separado.",
          "**Distribución normal**: la campana. Muchos métodos la asumen; casi ningún dato de negocio la cumple.",
        ] },
        { t: "nota", et: "Acuerdos de servicio", x: "Un promedio de entrega de 40 minutos suena bien, pero si el p90 es de 3 horas, uno de cada diez clientes está furioso. Los compromisos se miden en percentiles, no en promedios." },
        { t: "ej", x: "Haz el histograma del tiempo entre pedidos de un mismo cliente. La forma te dice si tienes clientes de rutina o de ocasión." },
        { t: "clave", x: "Primero la forma, después el resumen. Un promedio sin distribución es media respuesta." },
      ],
    },
    {
      id: "m7s3",
      titulo: "Correlación no es causalidad",
      objetivo: "No provocar decisiones caras con una coincidencia.",
      bloques: [
        { t: "p", x: "Dos series pueden moverse juntas por tres razones: una causa a la otra, ambas responden a un tercer factor, o es casualidad. Los datos solos no distinguen entre las tres." },
        { t: "p", x: "El ejemplo del oficio: «los clientes que usan la app gastan el doble». Puede que la app haga gastar más, o puede que los clientes que ya gastaban mucho sean justamente los que se molestan en instalar la app. La decisión de invertir en la app cambia por completo según cuál sea." },
        { t: "ul", x: [
          "**Variable oculta**: el calor sube el helado y los ahogamientos; el helado no ahoga a nadie.",
          "**Causalidad invertida**: ¿el descuento trajo al cliente o el cliente que iba a comprar usó el descuento?",
          "**Sesgo de selección**: mides solo a los que se quedaron, no a los que se fueron.",
        ] },
        { t: "nota", et: "Cómo se dice bien", x: "Cambia «la app aumenta el gasto» por «los usuarios de la app gastan más; para saber si la app lo causa habría que compararlos con un grupo parecido que no la use». Es más largo y es honesto." },
        { t: "ej", x: "Busca dos métricas de tu negocio que suban juntas y escribe tres explicaciones alternativas para esa coincidencia. Este ejercicio es el que separa a un analista de un generador de gráficas." },
        { t: "clave", x: "Antes de afirmar que A causa B, pregunta qué tercera cosa podría estar moviendo a las dos." },
      ],
    },
    {
      id: "m7s4",
      titulo: "Muestras e incertidumbre",
      objetivo: "Poner margen de error a lo que reportas.",
      bloques: [
        { t: "p", x: "Casi nunca mides todo: mides una parte y estimas el resto. Una muestra chica da un número que baila. Reportarlo como si fuera exacto es el error más caro de la estadística aplicada." },
        { t: "p", x: "Regla práctica: con menos de 30 observaciones, no hables de porcentajes. «El 66% de nuestros clientes prefiere el reparto nocturno» suena sólido hasta que ves que eran 3 de 5 personas." },
        { t: "ul", x: [
          "**Intervalo de confianza**: el rango donde probablemente está el valor real. Repórtalo: «12% ± 3%».",
          "**Tamaño de muestra**: cuadruplicarlo reduce el error a la mitad, no a la cuarta parte.",
          "**Muestra sesgada**: encuestar solo a quien contesta encuestas mide a quien contesta encuestas.",
        ] },
        { t: "nota", et: "En la práctica", x: "Cuando te den un porcentaje, pide siempre el denominador. «Subió 50%» puede ser de 2 a 3 pedidos. Muestra el numerador y el denominador juntos y nadie se confunde." },
        { t: "ej", x: "Toma cualquier porcentaje de un reporte tuyo y escríbelo con su denominador al lado. Decide si sigues cómodo presentándolo." },
        { t: "clave", x: "Todo porcentaje viaja con su denominador. Toda estimación viaja con su margen." },
      ],
    },
    {
      id: "m7s5",
      titulo: "Pruebas A/B y significancia",
      objetivo: "Decidir si una diferencia es real o es ruido.",
      bloques: [
        { t: "p", x: "Una prueba A/B compara dos versiones repartiendo a la gente al azar. El azar es lo que hace comparable a los dos grupos: sin él, vuelves al problema de la sesión 3." },
        { t: "res", head: ["concepto", "en cristiano"],
          rows: [["Hipótesis nula", "«no hay diferencia»; es lo que intentas descartar"], ["valor p", "qué tan probable era ver esto si no hubiera diferencia"], ["p < 0.05", "convención: poco probable que sea casualidad"], ["Potencia", "capacidad de detectar una diferencia real"], ["Efecto mínimo", "la diferencia que te importaría de verdad"]],
          meta: "5 conceptos de una prueba" },
        { t: "p", x: "Define **antes** de empezar: qué métrica, qué diferencia mínima te haría cambiar la decisión y cuántos días va a correr. Cambiar eso a mitad del camino, o parar la prueba en cuanto se ve bonita, invalida el resultado." },
        { t: "nota", et: "Lo que el valor p no dice", x: "Un p bajo no significa que el efecto sea grande ni importante. Con suficientes usuarios, una mejora de 0.1% sale «significativa» y no paga ni la junta donde la presentas. Mira siempre el tamaño del efecto." },
        { t: "ej", x: "Diseña en papel una prueba para La Higuera: ¿reparto gratis arriba de 500 pesos aumenta el ticket promedio? Define métrica, grupos, duración y qué diferencia te haría implementarlo." },
        { t: "clave", x: "Significativo ≠ importante. Define la diferencia que te importa antes de mirar los datos." },
      ],
    },
  ],
};

const M8 = {
  id: "m8", n: "08",
  titulo: "Python y pandas",
  resumen: "Donde SQL se queda corto: limpieza, repetición y automatización.",
  sesiones: [
    {
      id: "m8s1",
      titulo: "Por qué Python después de SQL",
      objetivo: "Saber cuándo cambiar de herramienta.",
      bloques: [
        { t: "p", x: "SQL es imbatible para traer y agregar datos. Python entra cuando necesitas repetir un proceso, limpiar con reglas complicadas, unir archivos sueltos, llamar una API o generar un reporte solo." },
        { t: "res", head: ["tarea", "herramienta"],
          rows: [["agregar millones de filas", "SQL"], ["unir 40 archivos de Excel", "Python"], ["mandar un reporte cada lunes", "Python"], ["explorar una tabla nueva", "SQL"], ["limpieza con reglas raras", "Python"], ["gráficas para presentar", "BI / Python"]],
          meta: "6 decisiones frecuentes" },
        { t: "ul", x: [
          "**pandas**: tablas en memoria. Es el 90% de tu trabajo en Python.",
          "**Jupyter**: cuaderno donde ves el resultado de cada paso. Ideal para explorar.",
          "**matplotlib / seaborn**: gráficas rápidas para ti, no necesariamente para presentar.",
          "**SQLAlchemy**: el puente para consultar tu base desde Python.",
        ] },
        { t: "nota", et: "Orden de aprendizaje", x: "No estudies Python entero antes de empezar. Con leer archivos, filtrar, agrupar, unir y exportar cubres casi todo lo que hace un analista. El resto lo aprendes cuando te haga falta." },
        { t: "clave", x: "SQL trae y resume. Python repite, limpia y automatiza. No compiten." },
      ],
    },
    {
      id: "m8s2",
      titulo: "DataFrames: cargar y mirar",
      objetivo: "Abrir datos y hacerte una idea en un minuto.",
      bloques: [
        { t: "p", x: "Un `DataFrame` es una tabla con nombre de columnas: la misma idea que ya conoces, ahora en memoria. Los primeros cuatro comandos son siempre los mismos." },
        { t: "py", cap: "Primer contacto", x: `import pandas as pd

df = pd.read_csv("pedidos.csv", parse_dates=["fecha"])

df.head(10)        # las primeras filas
df.shape           # (filas, columnas)
df.info()          # tipos y cuántos no nulos hay
df.describe()      # min, max, media, percentiles` },
        { t: "p", x: "`info()` es la auditoría de calidad de la sesión 6.3 en una línea: te dice el tipo de cada columna y cuántos valores no nulos tiene. Si una columna de dinero aparece como `object`, hay texto escondido dentro." },
        { t: "py", cap: "También lee directo de la base", x: `from sqlalchemy import create_engine

motor = create_engine("sqlite:///higuera.db")
df = pd.read_sql("SELECT * FROM pedidos WHERE estado = 'entregado'", motor)` },
        { t: "nota", et: "Buen hábito", x: "Filtra en SQL, no en pandas. Traer 5 millones de filas a memoria para quedarte con 20 mil es la forma más rápida de tronar tu computadora." },
        { t: "ej", x: "Exporta una consulta a CSV, cárgala con pandas y corre `info()` y `describe()`. Compara lo que ves con lo que esperabas." },
        { t: "clave", x: "`head`, `shape`, `info`, `describe`. Cuatro comandos y ya sabes con qué estás tratando." },
      ],
    },
    {
      id: "m8s3",
      titulo: "Filtrar, agrupar y unir en pandas",
      objetivo: "Traducir lo que ya sabes de SQL.",
      bloques: [
        { t: "p", x: "Todo lo que aprendiste tiene su equivalente. Si sabes SQL, pandas es cuestión de traducir sintaxis." },
        { t: "res", head: ["SQL", "pandas"],
          rows: [["WHERE total > 500", "df[df.total > 500]"], ["SELECT a, b", "df[['a','b']]"], ["ORDER BY total DESC", "df.sort_values('total', ascending=False)"], ["GROUP BY ciudad", "df.groupby('ciudad')"], ["COUNT(*)", ".size()"], ["JOIN", "pd.merge(a, b, on='id')"], ["LIMIT 10", "df.head(10)"]],
          meta: "7 equivalencias" },
        { t: "py", cap: "Un reporte completo", x: `resumen = (
    df[df.estado == "entregado"]
      .groupby("ciudad")
      .agg(pedidos=("id_pedido", "count"),
           venta=("total", "sum"),
           ticket=("total", "mean"))
      .round(2)
      .sort_values("venta", ascending=False)
      .reset_index()
)
print(resumen)` },
        { t: "res", head: ["ciudad", "pedidos", "venta", "ticket"],
          rows: [["Culiacán", "742", "381204.00", "513.75"], ["Mazatlán", "356", "156880.50", "440.68"], ["Los Mochis", "186", "74346.00", "399.71"]],
          meta: "3 filas · mismo resultado que en SQL" },
        { t: "ej", x: "Reproduce en pandas tu consulta favorita del módulo 3 y compara los dos resultados fila por fila. Si no coinciden, casi siempre es por nulos." },
        { t: "clave", x: "`groupby().agg()` es tu `GROUP BY`. `merge()` es tu `JOIN`. El resto es puntuación." },
      ],
    },
    {
      id: "m8s4",
      titulo: "Limpieza con pandas",
      objetivo: "Arreglar lo que SQL no alcanza a arreglar cómodo.",
      bloques: [
        { t: "py", cap: "Las operaciones de todos los días", x: `df = df.drop_duplicates(subset="id_pedido")
df["ciudad"] = df.ciudad.str.strip().str.title()
df["total"]  = pd.to_numeric(df.total, errors="coerce")
df["envio"]  = df.envio.fillna(0)

# marcar y revisar valores extremos en vez de borrarlos
limite = df.total.quantile(0.99)
df["revisar"] = df.total > limite` },
        { t: "ul", x: [
          "`errors='coerce'` convierte lo que no se puede leer en nulo: úsalo y luego cuenta cuántos salieron.",
          "`fillna(0)` solo si el nulo **significa** cero. Si significa «no sé», rellenarlo con cero inventa datos.",
          "`str.title()` normaliza mayúsculas; combínalo con `str.strip()` siempre.",
          "Marca los extremos con una columna booleana en vez de eliminarlos.",
        ] },
        { t: "nota", et: "Regla", x: "Nunca sobrescribas el archivo original. Trabaja en copias y guarda cada paso; si el reporte sale raro, quieres poder volver al dato crudo." },
        { t: "ej", x: "Toma un CSV desordenado, límpialo en cinco pasos y escribe al final cuántas filas entraron, cuántas salieron y por qué. Ese conteo es tu bitácora." },
        { t: "clave", x: "Limpiar es decidir, y toda decisión se documenta: cuántas filas cambiaron y con qué criterio." },
      ],
    },
    {
      id: "m8s5",
      titulo: "Automatizar un reporte",
      objetivo: "Que el reporte del lunes se haga solo.",
      bloques: [
        { t: "p", x: "El objetivo de aprender Python es dejar de repetirte. Un script que consulta, calcula y exporta convierte dos horas semanales en dos segundos." },
        { t: "py", cap: "Reporte semanal de punta a punta", x: `import pandas as pd
from sqlalchemy import create_engine
from datetime import date

motor = create_engine("sqlite:///higuera.db")

consulta = """
    SELECT ciudad, COUNT(*) AS pedidos, SUM(total) AS venta
    FROM pedidos
    WHERE estado = 'entregado'
      AND fecha >= DATE('now', '-7 day')
    GROUP BY ciudad
"""

df = pd.read_sql(consulta, motor)
df["ticket"] = (df.venta / df.pedidos).round(2)

archivo = f"reporte_{date.today():%Y-%m-%d}.xlsx"
df.to_excel(archivo, index=False, sheet_name="Semana")
print(f"Listo: {archivo} · {len(df)} ciudades")` },
        { t: "ul", x: [
          "Guarda la consulta en el script, no en tu memoria.",
          "Pon la fecha en el nombre del archivo: nunca sobrescribas historia.",
          "Imprime al final cuántas filas salieron: es tu alarma si algo viene vacío.",
          "Programa la ejecución con `cron` (Mac/Linux) o el Programador de tareas (Windows).",
        ] },
        { t: "ej", x: "Automatiza el reporte que más veces hayas hecho a mano. Cronometra cuánto tardabas antes y multiplícalo por 52: ese es el regalo que te acabas de hacer." },
        { t: "clave", x: "Si lo hiciste tres veces igual, automatízalo. La tercera vez es la señal." },
      ],
    },
  ],
};

const M9 = {
  id: "m9", n: "09",
  titulo: "Visualizar y contar la historia",
  resumen: "El número solo convence cuando se ve y se explica bien.",
  sesiones: [
    {
      id: "m9s1",
      titulo: "Elegir el gráfico correcto",
      objetivo: "Que la forma coincida con la pregunta.",
      bloques: [
        { t: "p", x: "No existe el gráfico bonito: existe el gráfico que contesta la pregunta. Primero define qué quieres mostrar, y el tipo sale solo." },
        { t: "res", head: ["quiero mostrar", "usa", "evita"],
          rows: [["evolución en el tiempo", "línea", "barras apiladas"], ["comparar categorías", "barras horizontales", "pastel"], ["composición de un total", "barras apiladas al 100%", "pastel con 8 rebanadas"], ["relación entre dos variables", "dispersión", "dos líneas con ejes distintos"], ["distribución", "histograma o caja", "solo el promedio"], ["un número clave", "el número, grande", "un medidor de velocímetro"]],
          meta: "6 preguntas, 6 formas" },
        { t: "p", x: "El pastel merece su mala fama: el ojo humano compara mal los ángulos. Con más de tres rebanadas, unas barras ordenadas de mayor a menor siempre se leen más rápido." },
        { t: "nota", et: "Prueba rápida", x: "Enseña tu gráfica cinco segundos y pregunta qué entendió la otra persona. Si tiene que estudiarla, la gráfica falló, no la persona." },
        { t: "ej", x: "Toma tres reportes que uses y decide si el tipo de gráfica corresponde a la pregunta. Cambia al menos uno." },
        { t: "clave", x: "Tiempo → línea. Comparación → barras. Relación → dispersión. Composición → barras apiladas, no pastel." },
      ],
    },
    {
      id: "m9s2",
      titulo: "Gráficas que no engañan",
      objetivo: "Quitar todo lo que estorbe y no exagerar diferencias.",
      bloques: [
        { t: "ul", x: [
          "**Eje en cero** para barras, sin excepción: cortarlo triplica visualmente diferencias mínimas.",
          "**Ordena** las categorías por valor, no por alfabeto, salvo que el orden signifique algo.",
          "**Etiqueta directo** sobre la línea o la barra en vez de mandar al lector a una leyenda lejana.",
          "**Un color de acento** y el resto en gris: el color debe señalar, no decorar.",
          "**Quita** rejillas dobles, sombras, 3D y bordes: cada pixel que borras aumenta la legibilidad.",
        ] },
        { t: "p", x: "El título es la parte más desperdiciada de una gráfica. «Ventas mensuales» no dice nada; «Las ventas cayeron 16% en julio por el cierre de Mazatlán» ya es el hallazgo. El título carga la conclusión; el gráfico, la evidencia." },
        { t: "nota", et: "Accesibilidad", x: "Uno de cada doce hombres no distingue rojo de verde. No codifiques información solo con color: usa también posición, forma o etiquetas." },
        { t: "ej", x: "Toma una gráfica tuya y bórrale todo lo que no sea dato. Después ponle un título que sea una frase completa con un verbo." },
        { t: "clave", x: "El título dice la conclusión, el gráfico la demuestra. Todo lo demás sobra." },
      ],
    },
    {
      id: "m9s3",
      titulo: "Tableros: Power BI, Tableau, Looker Studio",
      objetivo: "Construir algo que la gente use sin ti.",
      bloques: [
        { t: "res", head: ["herramienta", "fuerte en", "considera"],
          rows: [["Power BI", "empresas con Microsoft", "DAX tiene su curva"], ["Tableau", "exploración visual", "licencia cara"], ["Looker Studio", "gratis, conecta a Google", "se atora con datos grandes"], ["Metabase", "código abierto, muy directo", "menos personalizable"]],
          meta: "4 opciones habituales" },
        { t: "p", x: "La herramienta importa menos que el diseño. Un tablero se lee de arriba-izquierda a abajo-derecha: pon los dos o tres números que definen el negocio arriba, el detalle abajo, y los filtros siempre en el mismo lugar." },
        { t: "ul", x: [
          "Máximo 5 o 6 elementos por pantalla. Si necesitas más, es otro tablero.",
          "Fecha de última actualización visible: sin eso nadie sabe si puede confiar.",
          "Toda métrica con su definición a la mano, aunque sea en un tooltip.",
          "Si nadie lo abrió en un mes, apágalo. Un tablero muerto ensucia y confunde.",
        ] },
        { t: "nota", et: "Antes de construir", x: "Pregúntale a quien lo va a usar: «¿qué decisión vas a tomar con esto?». Si no hay decisión, no hagas el tablero: manda un número por mensaje y ahórrense los dos el trabajo." },
        { t: "ej", x: "Diseña en papel el tablero de La Higuera: tres números arriba, evolución mensual al centro, top de productos abajo y filtros de ciudad y fecha." },
        { t: "clave", x: "Un tablero sin decisión asociada es decoración cara." },
      ],
    },
    {
      id: "m9s4",
      titulo: "Métricas y KPI que valen la pena",
      objetivo: "Elegir pocos números y definirlos bien.",
      bloques: [
        { t: "p", x: "Un KPI es una métrica que alguien vigila porque puede actuar sobre ella. Si nadie puede moverla, es un dato curioso, no un indicador." },
        { t: "res", head: ["métrica", "qué mide", "cuidado con"],
          rows: [["Ticket promedio", "venta / pedidos", "los extremos: mira la mediana"], ["Recurrencia", "% que vuelve a comprar", "definir bien la ventana"], ["Tasa de cancelación", "cancelados / totales", "no mezclar motivos"], ["Margen", "venta − costo", "olvidar el costo de reparto"], ["Frecuencia", "pedidos por cliente / mes", "el promedio esconde dos poblaciones"]],
          meta: "5 KPI de una tienda con reparto" },
        { t: "ul", x: [
          "Métrica **accionable**: alguien puede hacer algo mañana para moverla.",
          "Métrica **comparable**: contra el periodo anterior o contra una meta.",
          "Métrica **definida por escrito**: en un glosario que todos consulten.",
          "Cuidado con optimizar una sola: bajar cancelaciones rechazando pedidos difíciles también baja la venta.",
        ] },
        { t: "nota", et: "Ley de Goodhart", x: "Cuando una medida se convierte en objetivo, deja de ser una buena medida. Acompaña siempre un KPI con su contrapeso: velocidad con calidad, volumen con margen." },
        { t: "ej", x: "Escribe el glosario de cinco métricas de tu negocio: nombre, fórmula exacta, periodo y quién es responsable." },
        { t: "clave", x: "Pocos indicadores, definidos por escrito y con contrapeso." },
      ],
    },
    {
      id: "m9s5",
      titulo: "Presentar hallazgos",
      objetivo: "Que la junta termine en una decisión.",
      bloques: [
        { t: "p", x: "Nadie quiere ver tu proceso. Empieza por la conclusión, sostenla con dos o tres evidencias y cierra con la recomendación. Si preguntan cómo lo hiciste, ahí tienes el anexo." },
        { t: "h", x: "Estructura de cuatro partes" },
        { t: "ul", x: [
          "**Hallazgo**: «Las cancelaciones en Mazatlán subieron a 11.4%, casi el doble que en Culiacán».",
          "**Evidencia**: una gráfica, el periodo y el volumen detrás del porcentaje.",
          "**Causa probable**: «el 70% se cancela después de 40 minutos de espera».",
          "**Recomendación**: qué hacer, quién y cuándo se vuelve a medir.",
        ] },
        { t: "p", x: "Di también lo que **no** sabes. Un analista que aclara los límites de su análisis gana más confianza que uno que suena seguro de todo, y sobrevive mejor cuando alguien encuentra el hueco." },
        { t: "nota", et: "Antes de mandar", x: "Léelo como si fueras quien recibe: ¿está el periodo?, ¿está el denominador?, ¿se entiende sin ti presente?, ¿queda claro qué hacer el lunes?" },
        { t: "ej", x: "Escribe el hallazgo de tu último análisis en cuatro frases siguiendo la estructura de arriba. Si no cabe en cuatro, todavía no lo entiendes bien." },
        { t: "clave", x: "Conclusión primero, evidencia después, recomendación al final. El método va en el anexo." },
      ],
    },
  ],
};

const M10 = {
  id: "m10", n: "10",
  titulo: "Del aprendizaje al trabajo",
  resumen: "Proyecto, portafolio, entrevista y hacia dónde seguir.",
  sesiones: [
    {
      id: "m10s1",
      titulo: "Proyecto final de punta a punta",
      objetivo: "Juntar todo el curso en un solo entregable.",
      bloques: [
        { t: "p", x: "Un proyecto completo vale más que diez ejercicios sueltos, porque demuestra el ciclo entero. Elige datos que te importen: tus propios números, datos abiertos de tu ciudad o un negocio que conozcas." },
        { t: "h", x: "Las siete etapas" },
        { t: "ul", x: [
          "**Pregunta**: una sola, escrita con definición, periodo y corte.",
          "**Datos**: de dónde salen, cuántas filas, qué periodo cubren.",
          "**Auditoría**: duplicados, nulos, rangos y categorías (módulo 6).",
          "**Consultas**: en CTE, con nombres legibles y comentarios.",
          "**Análisis**: la métrica, cortada por dos o tres dimensiones.",
          "**Visual**: dos o tres gráficas con títulos que sean conclusiones.",
          "**Recomendación**: qué haría el negocio distinto la semana que entra.",
        ] },
        { t: "nota", et: "Alcance", x: "Un proyecto terminado y chico gana siempre a uno ambicioso a medias. Una pregunta, tres tablas, dos semanas." },
        { t: "ej", x: "Escribe hoy la pregunta de tu proyecto y de dónde vas a sacar los datos. Ponle fecha de entrega en el calendario." },
        { t: "clave", x: "Termina algo pequeño. Lo terminado es lo que puedes enseñar." },
      ],
    },
    {
      id: "m10s2",
      titulo: "Portafolio: que se vea tu trabajo",
      objetivo: "Convertir el proyecto en algo que se pueda revisar en 3 minutos.",
      bloques: [
        { t: "p", x: "Quien revisa candidatos tiene poco tiempo. Tu repositorio tiene que contar la historia solo, sin que tú lo expliques." },
        { t: "ul", x: [
          "**README primero**: pregunta, datos, hallazgo y una imagen del resultado. Arriba de todo.",
          "**SQL legible**: archivos con nombre (`01_auditoria.sql`, `02_metricas.sql`), comentados.",
          "**Resultado visible**: una captura del tablero o de la gráfica en el propio README.",
          "**Sin datos privados**: usa datos abiertos o inventados, nunca información real de un empleo.",
          "**Dos o tres proyectos bien terminados** valen más que diez repos vacíos.",
        ] },
        { t: "nota", et: "Lo que más se nota", x: "Un README que empieza con «Este análisis encontró que…» y una gráfica clara pesa más que cualquier lista de tecnologías. Estás demostrando que sabes comunicar, que es la mitad del trabajo." },
        { t: "ej", x: "Escribe el README de tu proyecto antes de terminarlo. Te va a obligar a definir el hallazgo y a no perderte en el camino." },
        { t: "clave", x: "El portafolio no muestra que sabes SQL: muestra que sabes contestar una pregunta de negocio." },
      ],
    },
    {
      id: "m10s3",
      titulo: "Entrevistas: lo que sí preguntan",
      objetivo: "Llegar preparado a la prueba técnica.",
      bloques: [
        { t: "p", x: "Las pruebas de SQL para analista se repiten muchísimo. Practica estas siete y vas a reconocer el 80% de lo que te pongan." },
        { t: "ul", x: [
          "Segundo salario más alto por departamento (ventana + CTE).",
          "Top N por grupo (`ROW_NUMBER` con `PARTITION BY`).",
          "Clientes que compraron en enero pero no en febrero (`LEFT JOIN` + `IS NULL`).",
          "Duplicados en una tabla (`GROUP BY … HAVING COUNT(*) > 1`).",
          "Crecimiento mes contra mes (`LAG`).",
          "Diferencia entre `WHERE` y `HAVING`, y entre `INNER` y `LEFT JOIN`.",
          "Qué es un `NULL` y por qué rompe los filtros negativos.",
        ] },
        { t: "p", x: "En la parte de negocio esperan otra cosa: cuenta un análisis tuyo con el formato hallazgo → evidencia → recomendación, y admite qué no pudiste concluir. Esa honestidad se lee como criterio." },
        { t: "nota", et: "Durante la prueba", x: "Piensa en voz alta y pregunta por las definiciones antes de escribir. Un candidato que pregunta «¿los cancelados cuentan?» ya demostró más criterio que uno que escribe rápido y adivina." },
        { t: "ej", x: "Resuelve las siete de arriba sobre La Higuera y guárdalas en un archivo. Es tu repaso de la noche anterior." },
        { t: "clave", x: "Preguntan poco SQL raro y mucho criterio. Piensa en voz alta y aclara definiciones." },
      ],
    },
    {
      id: "m10s4",
      titulo: "Hacia dónde seguir",
      objetivo: "Elegir la siguiente especialidad con los ojos abiertos.",
      bloques: [
        { t: "res", head: ["camino", "de qué se trata", "qué estudiar después"],
          rows: [["Analista sénior", "más negocio, menos consulta", "métricas, experimentos, comunicación"], ["Analytics engineer", "construir el modelo de datos", "dbt, Git, almacenes en la nube"], ["Ingeniero de datos", "las tuberías y la infraestructura", "Python, Airflow, sistemas distribuidos"], ["Científico de datos", "predecir y modelar", "estadística, machine learning"], ["BI / tableros", "que la empresa se vea a sí misma", "Power BI o Tableau a fondo"]],
          meta: "5 rutas desde el mismo punto" },
        { t: "p", x: "Todas parten del mismo lugar: saber traducir una pregunta a datos y devolver una respuesta confiable. Lo que cambia es qué tanto te acercas a la infraestructura o al negocio." },
        { t: "ul", x: [
          "Práctica constante: SQL se olvida rápido si no lo escribes cada semana.",
          "Datos abiertos de tu ciudad o del INEGI para practicar con cosas reales.",
          "Lee reportes de otros y fíjate en cómo definieron las métricas.",
          "Enseña lo que aprendiste: explicar un `LEFT JOIN` es la prueba de que lo entendiste.",
        ] },
        { t: "clave", x: "Ya sabes traducir preguntas a datos. Lo demás es elegir hacia qué lado profundizas." },
      ],
    },
  ],
};

const CURSO = [M1, M2, M3, M4, M5, M6, M7, M8, M9, M10];
const TODAS = CURSO.flatMap((m) => m.sesiones.map((s) => ({ ...s, mod: m })));

/* ══════════════════════════════════════════════════════════
   RESALTADO Y TEXTO
   ══════════════════════════════════════════════════════════ */

const SQL_KW = ["SELECT","FROM","WHERE","GROUP\\s+BY","ORDER\\s+BY","PARTITION\\s+BY","HAVING","LIMIT","OFFSET","INNER\\s+JOIN","LEFT\\s+JOIN","RIGHT\\s+JOIN","FULL\\s+JOIN","CROSS\\s+JOIN","JOIN","ON","AS","AND","OR","NOT","IN","IS","NULL","LIKE","BETWEEN","CASE","WHEN","THEN","ELSE","END","DISTINCT","UNION\\s+ALL","UNION","INTERSECT","EXCEPT","WITH","OVER","ROWS","UNBOUNDED","PRECEDING","FOLLOWING","CURRENT\\s+ROW","CREATE","TABLE","INDEX","INSERT","INTO","VALUES","UPDATE","SET","DELETE","BEGIN","COMMIT","ROLLBACK","PRIMARY\\s+KEY","FOREIGN\\s+KEY","REFERENCES","CHECK","UNIQUE","DEFAULT","EXPLAIN","QUERY\\s+PLAN","NULLS\\s+LAST","INTERVAL","DESC","ASC","TOP"];
const SQL_FN = ["COUNT","SUM","AVG","MIN","MAX","ROUND","COALESCE","ROW_NUMBER","DENSE_RANK","RANK","LAG","LEAD","TRIM","LTRIM","RTRIM","LOWER","UPPER","REPLACE","SUBSTR","LENGTH","STRFTIME","DATE_TRUNC","DATE_FORMAT","DATEDIFF","JULIANDAY","CURDATE","CAST","CONCAT","DATE","YEAR"];
const SQL_TP = ["INTEGER","TEXT","REAL","BOOLEAN","VARCHAR","TIMESTAMP","CURRENT_DATE"];

const RX_SQL = new RegExp(
  "(--[^\\n]*)" +
  "|('(?:[^']|'')*')" +
  "|\\b(" + SQL_FN.join("|") + ")(?=\\s*\\()" +
  "|\\b(" + SQL_KW.join("|") + ")\\b" +
  "|\\b(" + SQL_TP.join("|") + ")\\b" +
  "|\\b(\\d+(?:\\.\\d+)?)\\b", "gi");

const RX_PY = new RegExp(
  "(#[^\\n]*)" +
  "|(\"\"\"[\\s\\S]*?\"\"\"|'[^'\\n]*'|\"[^\"\\n]*\")" +
  "|\\b(read_csv|read_sql|groupby|agg|merge|sort_values|drop_duplicates|fillna|to_numeric|quantile|create_engine|to_excel|describe|info|head|shape|round|reset_index|print|len|str|strip|title)(?=\\s*\\()" +
  "|\\b(import|from|as|def|return|if|else|elif|for|in|with|not|and|or|True|False|None|lambda|class)\\b" +
  "|\\b(\\d+(?:\\.\\d+)?)\\b", "g");

function pintar(codigo, lenguaje) {
  const rx = lenguaje === "py" ? RX_PY : RX_SQL;
  rx.lastIndex = 0;
  const salida = [];
  let ultimo = 0, m, i = 0;
  while ((m = rx.exec(codigo)) !== null) {
    if (m.index > ultimo) salida.push(codigo.slice(ultimo, m.index));
    const clase = m[1] ? "c" : m[2] ? "s" : m[3] ? "f" : m[4] ? "k" : m[5] ? "f" : "n";
    salida.push(<span className={clase} key={i++}>{m[0]}</span>);
    ultimo = m.index + m[0].length;
  }
  if (ultimo < codigo.length) salida.push(codigo.slice(ultimo));
  return salida;
}

function enLinea(texto) {
  return String(texto).split(/(\*\*[^*]+\*\*|`[^`]+`)/g).map((p, i) => {
    if (p.length > 4 && p.startsWith("**") && p.endsWith("**"))
      return <strong key={i} style={{ fontWeight: 600 }}>{p.slice(2, -2)}</strong>;
    if (p.length > 2 && p.startsWith("`") && p.endsWith("`"))
      return <code className="chip" key={i}>{p.slice(1, -1)}</code>;
    return p;
  });
}

function minutos(sesion) {
  const n = sesion.bloques.reduce((suma, b) => {
    if (Array.isArray(b.x)) return suma + b.x.join(" ").length;
    if (b.t === "res") return suma + 120;
    return suma + String(b.x || "").length;
  }, 0);
  return Math.max(3, Math.round(n / 700));
}

/* ══════════════════════════════════════════════════════════
   PIEZAS
   ══════════════════════════════════════════════════════════ */

function Hoja({ head, rows, meta }) {
  return (
    <div className="hoja">
      <div className="hoja-in">
        <table>
          <thead><tr>{head.map((h, i) => <th key={i}>{h}</th>)}</tr></thead>
          <tbody>
            {rows.map((fila, i) => (
              <tr key={i}>
                {fila.map((celda, j) => (
                  <td key={j} className={celda === "—" ? "nulo" : ""}>
                    {celda === "—" ? "NULL" : celda}
                  </td>
                ))}
              </tr>
            ))}
          </tbody>
        </table>
      </div>
      {meta && <div className="hoja-pie">{meta}</div>}
    </div>
  );
}

function Bloque({ b }) {
  switch (b.t) {
    case "p":
      return <p className="p">{enLinea(b.x)}</p>;
    case "h":
      return <h3 className="h2">{b.x}</h3>;
    case "ul":
      return <ul className="ul">{b.x.map((li, i) => <li key={i}>{enLinea(li)}</li>)}</ul>;
    case "sql":
    case "py":
      return (
        <div>
          {b.cap && <div className="cap">{b.cap}</div>}
          <pre className="code">{pintar(b.x, b.t)}</pre>
        </div>
      );
    case "res":
      return <Hoja head={b.head} rows={b.rows} meta={b.meta} />;
    case "nota":
      return (
        <aside className="nota">
          <span className="et">{b.et || "Nota"}</span>
          {enLinea(b.x)}
        </aside>
      );
    case "ej":
      return (
        <div className="ej">
          <span className="et">Ejercicio</span>
          {enLinea(b.x)}
          {b.pista && <span className="pista">Pista: {enLinea(b.pista)}</span>}
        </div>
      );
    case "clave":
      return (
        <div className="clave">
          <span className="et">Clave</span>
          {enLinea(b.x)}
        </div>
      );
    default:
      return null;
  }
}

function Indice({ idx, hechas, abiertos, alternar, ir }) {
  return (
    <>
      <h4>Contenido · {TODAS.length} sesiones</h4>
      {CURSO.map((m) => {
        const listas = m.sesiones.filter((s) => hechas.includes(s.id)).length;
        const abierto = abiertos.includes(m.id);
        return (
          <div className="mod" key={m.id}>
            <button className="mod-b" onClick={() => alternar(m.id)}
                    aria-expanded={abierto}>
              <span className="mod-n">{m.n}</span>
              <span className="mod-t">{m.titulo}</span>
              <span className="mod-c">{listas}/{m.sesiones.length}</span>
            </button>
            {abierto && m.sesiones.map((s) => {
              const posicion = TODAS.findIndex((t) => t.id === s.id);
              const aqui = posicion === idx;
              const hecha = hechas.includes(s.id);
              return (
                <button key={s.id}
                        className={"ses-b" + (aqui ? " aqui" : "") + (hecha ? " hecha" : "")}
                        aria-current={aqui ? "true" : undefined}
                        onClick={() => ir(posicion)}>
                  <span className="tic">{hecha ? "✓" : ""}</span>
                  <span>{s.titulo}</span>
                </button>
              );
            })}
          </div>
        );
      })}
    </>
  );
}

const CSS_EXTRA = `
.solo-movil { display: none !important; }
@media (max-width: 900px) {
  .solo-movil { display: inline-block !important; }
  .solo-esc { display: none !important; }
}
`;

/* ══════════════════════════════════════════════════════════
   APLICACIÓN
   ══════════════════════════════════════════════════════════ */

export default function Renglon() {
  const [idx, setIdx] = useState(0);
  const [hechas, setHechas] = useState([]);
  const [tema, setTema] = useState("papel");
  const [tam, setTam] = useState(1);
  const [enfoque, setEnfoque] = useState(false);
  const [railAbierto, setRailAbierto] = useState(false);
  const [abiertos, setAbiertos] = useState(["m1"]);
  const [cargado, setCargado] = useState(false);

  const sesion = TODAS[idx];
  const t = TEMAS[tema];

  /* memoria entre visitas */
  useEffect(() => {
    let vivo = true;
    (async () => {
      try {
        const r = await window.storage.get("renglon:avance");
        if (vivo && r && r.value) {
          const d = JSON.parse(r.value);
          if (Array.isArray(d.hechas)) setHechas(d.hechas);
          if (typeof d.idx === "number" && d.idx >= 0 && d.idx < TODAS.length) {
            setIdx(d.idx);
            setAbiertos([TODAS[d.idx].mod.id]);
          }
          if (d.tema && TEMAS[d.tema]) setTema(d.tema);
          if (typeof d.tam === "number") setTam(d.tam);
        }
      } catch (e) { /* primera visita o sin almacenamiento */ }
      if (vivo) setCargado(true);
    })();
    return () => { vivo = false; };
  }, []);

  useEffect(() => {
    if (!cargado) return;
    const id = setTimeout(() => {
      try {
        const r = window.storage.set("renglon:avance", JSON.stringify({ hechas, idx, tema, tam }));
        if (r && r.catch) r.catch(() => {});
      } catch (e) { /* sin almacenamiento: el avance dura la sesión */ }
    }, 350);
    return () => clearTimeout(id);
  }, [hechas, idx, tema, tam, cargado]);

  /* navegación */
  const ir = (n) => {
    if (n < 0 || n >= TODAS.length) return;
    setIdx(n);
    setRailAbierto(false);
    setAbiertos((a) => (a.includes(TODAS[n].mod.id) ? a : [...a, TODAS[n].mod.id]));
    if (typeof window !== "undefined") {
      window.scrollTo({ top: 0, behavior: "auto" });
      if (document.documentElement) document.documentElement.scrollTop = 0;
    }
  };

  useEffect(() => {
    const tecla = (e) => {
      const activo = document.activeElement;
      if (activo && /INPUT|TEXTAREA/.test(activo.tagName)) return;
      if (e.key === "ArrowRight") ir(idx + 1);
      if (e.key === "ArrowLeft") ir(idx - 1);
    };
    window.addEventListener("keydown", tecla);
    return () => window.removeEventListener("keydown", tecla);
  }, [idx]);

  const alternarHecha = () => {
    setHechas((h) => (h.includes(sesion.id) ? h.filter((x) => x !== sesion.id) : [...h, sesion.id]));
  };

  const vars = useMemo(() => ({
    "--bg": t.bg, "--panel": t.panel, "--stripe": t.stripe, "--code": t.code,
    "--rule": t.rule, "--ruleFaint": t.ruleFaint, "--ink": t.ink, "--soft": t.soft,
    "--faint": t.faint, "--acc": t.acc, "--accSoft": t.accSoft, "--ocre": t.ocre,
    "--ocreSoft": t.ocreSoft, "--num": t.num, "--fn": t.fn, "--sombra": t.sombra,
    "--serif": SERIF, "--sans": SANS, "--mono": MONO,
    "--fs": TAMANOS[tam].px + "px",
  }), [t, tam]);

  const listo = hechas.includes(sesion.id);
  const pct = Math.round((hechas.length / TODAS.length) * 100);
  const nSesion = sesion.mod.sesiones.findIndex((s) => s.id === sesion.id) + 1;
  const previa = idx > 0 ? TODAS[idx - 1] : null;
  const siguiente = idx < TODAS.length - 1 ? TODAS[idx + 1] : null;

  return (
    <div className="app" style={vars}>
      <style>{CSS + CSS_EXTRA}</style>

      <header className="top">
        <div className="top-in">
          <button className="pill solo-movil" onClick={() => setRailAbierto((v) => !v)}>
            Índice
          </button>
          <div className="marca">
            <b>Renglón</b>
            <span>Analista de datos</span>
          </div>
          <div className="ctrl">
            <span className="avance">{hechas.length}/{TODAS.length} · {pct}%</span>
            <div className="sizer" role="group" aria-label="Tamaño del texto">
              {TAMANOS.map((z, i) => (
                <button key={z.id} onClick={() => setTam(i)} className={i === tam ? "on" : ""}
                        title={z.nota} aria-label={"Texto " + z.nota}
                        style={{ fontSize: 11 + i * 3 }}>{z.et}</button>
              ))}
            </div>
            <button className="pill" onClick={() => setTema(tema === "papel" ? "noche" : "papel")}>
              {tema === "papel" ? "Noche" : "Papel"}
            </button>
            <button className={"pill solo-esc" + (enfoque ? " on" : "")}
                    onClick={() => setEnfoque((v) => !v)}>
              Enfoque
            </button>
          </div>
        </div>
        <div className="tally"><i style={{ width: pct + "%" }} /></div>
      </header>

      <div className={"wrap" + (enfoque ? " solo" : "")}>
        {!enfoque && (
          <nav className={"rail" + (railAbierto ? " abierto" : "")}>
            <Indice idx={idx} hechas={hechas} abiertos={abiertos} ir={ir}
                    alternar={(id) => setAbiertos((a) => a.includes(id) ? a.filter((x) => x !== id) : [...a, id])} />
          </nav>
        )}
        {railAbierto && !enfoque && <div className="velo" onClick={() => setRailAbierto(false)} />}

        <main className="stage">
          <article className={"paper" + (enfoque ? " ancho" : "")} key={sesion.id}>
            <div className="eyebrow">
              <span>Módulo {sesion.mod.n} · {sesion.mod.titulo}</span>
              <i>/</i>
              <span>Sesión {nSesion} de {sesion.mod.sesiones.length}</span>
              <i>/</i>
              <span>{minutos(sesion)} min</span>
            </div>

            <h1 className="h1">{sesion.titulo}</h1>
            <p className="obj">{sesion.objetivo}</p>

            {sesion.bloques.map((b, i) => <Bloque b={b} key={i} />)}

            <div className="pie">
              <button className={"marcar" + (listo ? " on" : "")} onClick={alternarHecha}>
                <span>{listo ? "✓" : "○"}</span>
                {listo ? "Sesión completada" : "Marcar como completada"}
              </button>

              <div className="saltos">
                <button className="salto" disabled={!previa} onClick={() => ir(idx - 1)}>
                  <small>Anterior</small>
                  <span>{previa ? previa.titulo : "Estás al inicio"}</span>
                </button>
                <button className="salto der" disabled={!siguiente} onClick={() => ir(idx + 1)}>
                  <small>Siguiente</small>
                  <span>{siguiente ? siguiente.titulo : "Terminaste el curso"}</span>
                </button>
              </div>

              <p className="teclas">Muévete con las flechas ← →</p>
            </div>
          </article>
        </main>
      </div>
    </div>
  );
}
