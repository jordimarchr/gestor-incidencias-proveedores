GESTOR DE INCIDENCIAS CON PROVEEDORES (COMPRAS)
Proyecto final - Microcredencial UC3M: IA Generativa para Todos
Estudiante: Jordi March

QUÉ HACE
Lee el texto de una incidencia con un proveedor (correo o nota del almacén) y:
1) [LLM] extrae y clasifica con Qwen3-4B: proveedor, nº de pedido, material, tipo (6 etiquetas),
   gravedad (alta/media/baja), motivo de la gravedad y resumen, en JSON;
2) [Python] valida la salida: etiquetas permitidas y borra el nº de pedido si no aparece en el texto;
3) [Python] propone acción y plazo con una tabla de reglas de Compras (aquí no interviene el modelo)
   y avisa si hay que escalar;
4) [LLM] redacta con Qwen3-4B un borrador de respuesta al proveedor, editable y copiable
   (lo revisa una persona antes de enviarlo);
5) [Python] registra la incidencia (ID, estado, fecha límite) y hace el seguimiento en vista Kanban o Tabla.
Idea de diseño: el LLM para entender y redactar; el código para el formato y las decisiones.

ARCHIVOS
- gestor_incidencias_proveedores.ipynb: el notebook del proyecto (código, explicaciones e interfaz).
- incidencias_sinteticas.csv: dataset propio, 20 incidencias inventadas y etiquetadas a mano
  (columnas id, texto, tipo_real, gravedad_real). El notebook ya lo lleva dentro y lo vuelve a crear,
  así que no hace falta subirlo a Colab.

CÓMO REPLICARLO PASO A PASO
1. Abre https://colab.research.google.com > Archivo > Subir cuaderno > gestor_incidencias_proveedores.ipynb.
2. Entorno de ejecución > Cambiar tipo de entorno de ejecución > GPU T4 > Guardar.
3. Entorno de ejecución > Ejecutar todo. Tarda unos 5 minutos la primera vez (descarga el modelo, ~8 GB).
   - El aviso rojo de pip sobre «datasets» no afecta.
   - No hace falta token de Hugging Face: el modelo es público. Si Colab avisa de que no existe
     el secreto HF_TOKEN, es solo un aviso y se puede ignorar.
4. La interfaz aparece debajo de la última celda (sección 9).
5. Pestaña «Nueva incidencia»: elige un ejemplo en la lista (o pega tu texto) > Analizar >
   Redactar respuesta al proveedor > Registrar incidencia.
6. Pestaña «Seguimiento»: indicadores, vista Kanban o Tabla y cambio de estado (Abierta, En curso, Cerrada).
7. La sección 8 hace una prueba de punta a punta sin interfaz, con un caso sin nº de pedido.
8. Las incidencias registradas se guardan en registro_incidencias.csv (panel Archivos de Colab;
   se puede descargar). Dura lo que el entorno de ejecución.

REQUISITOS
Google Colab con GPU T4. Se instalan en la primera celda: gradio==6.31.0, transformers==5.22.1, accelerate.
Modelo: Qwen/Qwen3-4B-Instruct-2507 (licencia Apache 2.0), se descarga de Hugging Face.

RESULTADOS
Con las 20 incidencias etiquetadas a mano, Qwen3-4B acierta el 100 % del tipo y el 80 % de la gravedad,
con 20/20 JSON válidos y 0 nº de pedido inventados (~9 s por incidencia en una T4).
Lo elegí tras compararlo con Qwen2.5-0.5B (el modelo base del curso), Qwen2.5-1.5B y Qwen2.5-7B:
fue el que mejor clasificó la gravedad.

LÍMITES
Datos pequeños y sintéticos (no encontre datos públicos reales de este dominio); el registro es un CSV que dura
lo que la sesión de Colab dure; sin conexión al correo ni al ERP; el plazo no tiene en cuenta el importe;
el modelo puede equivocarse: revisar siempre la ficha y el borrador antes de actuar.
