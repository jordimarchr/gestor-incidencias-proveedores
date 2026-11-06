GESTOR DE INCIDENCIAS CON PROVEEDORES (COMPRAS)
Proyecto final - Microcredencial UC3M "IA Generativa para Todos" (noviembre 2026) - Jordi March

QUE HACE
Lee el texto de una incidencia con un proveedor (correo o nota del almacen) y:
1) extrae y clasifica con un LLM (Qwen3-4B): proveedor, n.o de pedido, material, tipo (6 etiquetas),
   gravedad (alta/media/baja), motivo de la gravedad y resumen, en JSON;
2) valida la salida en Python (etiquetas permitidas; borra el n.o de pedido si no aparece en el texto);
3) propone accion y plazo con reglas fijas de Compras (sin LLM) y avisa si hay que escalar;
4) redacta un borrador de respuesta al proveedor, editable y copiable (lo revisa una persona);
5) registra la incidencia (ID, estado, fecha limite) y hace el seguimiento en vista Kanban o Tabla.
Todo en una interfaz Gradio.

ARCHIVOS
- gestor_incidencias_proveedores.ipynb : el notebook del proyecto (codigo, explicaciones e interfaz).
- incidencias_sinteticas.csv : dataset propio, 20 incidencias inventadas y etiquetadas a mano
  (columnas id, texto, tipo_real, gravedad_real). El notebook ya lo lleva dentro y lo vuelve a crear,
  asi que no hace falta subirlo a Colab.

COMO REPLICARLO PASO A PASO
1. Abre https://colab.research.google.com > Archivo > Subir cuaderno > gestor_incidencias_proveedores.ipynb.
2. Entorno de ejecucion > Cambiar tipo de entorno de ejecucion > GPU T4 > Guardar.
3. Entorno de ejecucion > Ejecutar todo. Tarda unos 5 minutos la primera vez (descarga el modelo, ~8 GB).
   El aviso rojo de pip sobre "datasets" no afecta.
4. La interfaz aparece debajo de la ultima celda (seccion 9). Si no se ve, abre el enlace http://127.0.0.1:...
   que imprime la celda.
5. Pestaña "Nueva incidencia": elige un ejemplo en la lista (o pega tu texto) > Analizar >
   Redactar respuesta al proveedor > Registrar incidencia.
6. Pestaña "Seguimiento": indicadores, vista Kanban o Tabla y cambio de estado (Abierta, En curso, Cerrada).
7. La seccion 8 hace una prueba de punta a punta sin interfaz con un caso sin n.o de pedido.

REQUISITOS
Google Colab con GPU T4 (gratis). Se instalan en la primera celda: gradio==6.31.0, transformers==5.22.1, accelerate.
Modelo: Qwen/Qwen3-4B-Instruct-2507 (licencia Apache 2.0), se descarga de Hugging Face sin token.
Los datos no salen de Colab: no se usa ninguna API externa.

RESULTADOS
Con las 20 incidencias etiquetadas a mano, Qwen3-4B acierta el 100 % del tipo y el 80 % de la gravedad,
con 20/20 JSON validos y 0 n.o de pedido inventados (~9 s por incidencia en una T4).

Antes de esta version compare 4 modelos Qwen (0.5B, 1.5B, 4B y 7B en 4 bits) y un baseline zero-shot (mDeBERTa);
Qwen3-4B fue el que mejor clasifico la gravedad. Esos experimentos no forman parte de esta entrega.

LIMITES
Datos pequeños y sinteticos (no hay datos publicos reales de este dominio); el registro es un CSV que dura lo
que la sesion de Colab; sin conexion a correo ni ERP; el plazo no tiene en cuenta el importe; el modelo puede
equivocarse: revisar siempre la ficha y el borrador antes de actuar.
