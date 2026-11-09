# Plan de Alimentación: app de seguimiento (Emmanuel y Melany)

Resumen de lo pedido y lo construido, para continuar el proyecto en Claude Code.

## Qué se pidió (en orden)

1. **Plan nutricional completo con calculadora**
   - Agua, carbohidratos, grasas, proteínas, verduras, frutas y lácteos.
   - Intercambios con su equivalencia.
   - Gasto calórico según estatura, peso, edad y actividad.
   - Una calculadora para registrar lo que se va comiendo durante el día.
2. **Usar el plan real de la nutricionista** (PDF de MAJO Nutrición Integral) e **incluir a la novia**, lo más parecido posible al formato del PDF.
3. **Fotos de la comida.** Tomar una foto y que la app identifique los alimentos y sume los intercambios.
4. **Plan de Melany** (PDF de MAJO), cargado tal cual.
5. **Subir la app a GitHub Pages.** Quedó pendiente: falta crear el repo y decidir qué hacer con los datos personales.

## Los dos planes (fuente: PDFs adjuntos, MAJO Nutrición Integral, 26/01/26)

### Emmanuel Barboza: 197,5 cm, 29 años
| Tiempo | Hora | Intercambios |
|---|---|---|
| Desayuno | 8:00 a.m. | Batido verde: 2 frutas + 1 vegetal |
| Almuerzo | 11:00 a.m. | 4 carbos, 10 proteínas (300 g), 2 grasas |
| Merienda | 3:00 p.m. | 2 carbos, 3 proteínas |
| Cena | 7:00 p.m. | 3 carbos, 7 proteínas (210 g), 2 grasas |

Control de citas (fecha · peso kg · grasa % · músculo kg · GV · H2O):
- 31/10/25 · 140,1 · 38 · 83 · 17 · 45
- 27/11/25 · 134,3 · 33,4 · 85,1 · 14 · 46,5
- 22/12/25 · 132,5 · 31,5 · 86,3 · 13 · 47,2
- 26/01/26 · 132,8 · 31,5 · 86,5 · 13 · 47,2

### Melany Garita Vásquez: 158 cm, 28 años
| Tiempo | Hora | Intercambios |
|---|---|---|
| Desayuno | 8:00 a.m. | 4 proteínas, 2 carbos, 2 grasas |
| Merienda | 10:00 a.m. | 1 fruta, 1 carbo |
| Almuerzo | 1:00 p.m. | 2 vegetales, 2 carbos, 5 proteínas (150 g), 1 grasa |
| Cena | 5:30 p.m. | 2 carbos, 3 proteínas (90 g) + **1 azúcar al día** |

Meta de grasa: 21–32,9 %. Control de citas:
- 31/10/25 · 71,3 · 36,2 · 43,2 · 5 · 45,2
- 27/11/25 · 70,7 · 35,9 · 43,2 · 5 · 45,4
- 22/12/25 · 70,1 · 33,7 · 44,1 · 5 · 47
- 26/01/26 · 72,1 · 35,3 · 44,3 · 5 · 45,9

### Recomendaciones (iguales para los dos)
Usar las medidas · Respetar los horarios · Aprenderse la lista de intercambios · No omitir alimentos / ejercicio 3 veces por semana · 2 L de agua al día (todo líquido cuenta) · 1 comida fuera del plan a la semana · Realizar actividad física · 1 taza de té de canela o jengibre al día.

### Valores por intercambio usados en la app
- Carbohidrato (harina): 80 kcal
- Proteína: 70–90 kcal (se usa 75; 30 g de carne cocida ≈ 7 g de proteína)
- Grasa: 45 kcal (5 g)
- Fruta: 60 kcal
- Vegetal: 25 kcal (1 taza crudo o 1/2 cocido)
- Lácteo: 90 kcal
- Azúcar: 60 kcal

La lista completa de equivalencias está en la página 2 de cualquiera de los PDFs (es la misma en los dos) y también dentro de `index.html`, en el arreglo `F`.

## Cómo está hecha la app (`index.html`)
Es un solo archivo HTML con JS puro, sin librerías y pensado primero para celular. Tiene un selector de persona arriba (Emmanuel / Melany) y 4 pestañas:

- **Hoy**
  - Registro por tiempo de comida, comparado con lo que le toca a cada uno.
  - Botones + / − por grupo y buscador de la lista de MAJO.
  - Contador de agua.
  - Casillas de té, ejercicio y comida libre.
  - Navegación por días.
- **Plan**
  - El plan con el formato del PDF y las recomendaciones.
  - Total de kcal y proteína, y comparación con el gasto estimado (Mifflin-St Jeor).
  - Botón "Editar plan" para cambiar intercambios y horarios.
- **Intercambios:** la lista completa de MAJO, separada por grupos.
- **Progreso**
  - Tabla de control de citas, con la opción de agregar o borrar mediciones.
  - Avance desde la primera cita.
  - Datos personales y cálculo de IMC, metabolismo basal y gasto diario.
  - Generador de plan con la misma estructura, por si se agrega otra persona.

**Foto de la comida:** se envía la imagen a Claude junto con la lista de intercambios. Claude devuelve JSON con los alimentos, la porción y los intercambios; la persona ajusta y agrega al tiempo de comida.

### Dependencias de Claude que hay que reemplazar al sacarla de Claude
- `window.claude.use('db')` + `use('user')`: guardan los datos en la cuenta.
  - Fuera de Claude la app cae a `localStorage`, que ya está implementado como respaldo.
  - Para sincronizar entre celulares hace falta un backend propio, por ejemplo Firebase o Supabase.
- `window.claude.use('sample')`: análisis de la foto.
  - Fuera de Claude la tarjeta de foto se oculta sola.
  - Para que funcione hace falta llamar a la API de Anthropic desde un backend, porque la API key no puede ir en el HTML público.

## Puntos que quedan por resolver
- **Datos personales:** los repos de GitHub son públicos, y la app trae nombres y mediciones corporales escritos en el código (`emmaDefault()` y `noviaDefault()`). Hay que decidir si se quitan antes de publicar. Lo recomendado es quitarlos.
- **Repo:** hay que crear `ebarboza96-cmyk/plan-nutricional` y activar GitHub Pages.
- **Interpretaciones que se hicieron del PDF:**
  - En el plan de Emmanuel el desayuno aparece vacío; se tomó el batido verde como desayuno.
  - En el plan de Emmanuel el almuerzo dice "11:00 p.m."; se puso como 11:00 a.m.
  - Yogurt YES se puso como 1 taza.
  - Fresas se puso como 1 taza.
  - Jugo de naranja Dos Pinos se puso como 200 ml y Jugo Welch's como 250 ml.
- En el PDF de Melany dice "MALANY"; en la app se usó "Melany".
- El PDF de Melany dice **"AYUNO"** junto a sus datos. Falta confirmar qué significa.
