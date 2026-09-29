# # 🏦 Proyección de un Certificado de Depósito a Término (CDT)

## 👥 Integrante:
-Samuel Iván Heredia Moya

Preparcial de **Programación II**. Módulo en Python que calcula y proyecta el saldo de un CDT a partir del monto inicial y el plazo en meses, con una interfaz gráfica hecha en **Gradio**.

## 📋 Descripción

Un CDT es una inversión en la que una persona entrega un monto a una entidad bancaria durante un plazo pactado. Cada mes el banco liquida el interés sobre el saldo y lo suma a este (el interés se capitaliza). El programa:

1. Solicita el monto inicial (COP) y el plazo (meses) y los valida.
2. Calcula la tasa mensual según el plazo.
3. Liquida mes a mes y muestra la tabla de proyección (mes 0 hasta el plazo).
4. Muestra el saldo final que recibirá el cliente, la tasa aplicada y el total de intereses ganados.

### Fórmulas

| Concepto | Fórmula |
|---|---|
| Tasa mensual (%) | `0.001695 * plazo + 0.0983` |
| Interés del mes | `saldo anterior * (tasa / 100)` |
| Saldo nuevo | `saldo anterior + interés` |
| Total de intereses | `saldo final - monto inicial` |

No se redondea durante los cálculos; solo se redondea a 2 decimales al **mostrar** los valores, en formato COP (ejemplo: `$ 1.014.330,07`).

## 🗂️ Estructura del repositorio

```
.
├── cdt_gradio.py                  # Código fuente (lógica + interfaz Gradio)
├── diagramas_cdt.md               # Análisis y diagramas (Mermaid)
├── diagramas_cdt_plantuml.md      # Diagramas en PlantUML
├── capturas/                      # Capturas de pantalla del funcionamiento
└── README.md
```

## 🧱 Arquitectura

| Clase | Capa | Responsabilidad |
|---|---|---|
| `CDT` | Reglas | Validar datos, calcular tasa, liquidar cada mes, proyectar y dar el resultado |
| `FilaProyeccion` | Reglas | Un renglón de la tabla (mes, intereses, saldo final) |
| `Presentacion` | Presentación | Capturar datos, mostrar tabla y resultado con Gradio |
| `main()` | Presentación | Crea la interfaz y la lanza |

## ⚙️ Requisitos

- Python 3.9 o superior
- [Gradio](https://www.gradio.app/)

```bash
pip install gradio
```

## ▶️ Cómo ejecutarlo

### Local

```bash
python cdt_gradio.py
```

Abre en el navegador la dirección que aparece en consola (por defecto `http://127.0.0.1:7860`).

### Google Colab

1. Crea un notebook nuevo.
2. En la primera celda ejecuta `!pip install gradio`.
3. En la siguiente celda pega el contenido de `cdt_gradio.py` y ejecútala. La interfaz se muestra con un enlace de acceso.

## ✅ Validaciones (RF2)

| Dato | Regla | Si es inválido |
|---|---|---|
| Monto | Número mayor a cero | Muestra el error y se vuelve a solicitar |
| Plazo | Entero mayor o igual a 1 | Muestra el error y se vuelve a solicitar |

Casos rechazados: monto `0`, negativo o texto; plazo `0`, negativo, decimal (`12.5`) o texto.

## 🧪 Casos de prueba

| Caso | Entrada | Resultado esperado |
|---|---|---|
| Ejemplo 1 | $ 1.000.000 / 12 meses | Tasa 0,1186 %, saldo final **$ 1.014.330,07** |
| Ejemplo 2 | $ 5.000.000 / 24 meses | Tasa 0,1390 %, saldo final **$ 5.169.468,90** |
| Plazo mínimo | $ 1.000.000 / 1 mes | Saldo final $ 1.000.999,95 |
| Plazo largo | $ 1.000.000 / 60 meses | Tasa 0,2000 %, saldo final $ 1.127.361,74 |
| Monto inválido | `0`, `-5`, `abc` | Error y se vuelve a solicitar |
| Plazo inválido | `0`, `-2`, `12.5`, `abc` | Error y se vuelve a solicitar |

> Nota: se acepta una tolerancia de $ 1 en el saldo final por el redondeo de la tasa.

## 📐 Diagramas

Los diagramas del diseño están en:

- [`diagramas_cdt.md`](diagramas_cdt.md): análisis del problema, clases, objetos, estructurados, secuencia y casos de prueba (Mermaid, se renderizan en GitHub).
- [`diagramas_cdt_plantuml.md`](diagramas_cdt_plantuml.md): los mismos diagramas en PlantUML.

Incluye: diagrama de clases, diagrama de objetos, diagramas estructurados (uno por método) y diagramas de secuencia (presentación, reglas y `main`).



**Asignatura:** Programación II  
**Herramienta:** Python en Google Colab + Gradio
