# Nodus — Simulador Financiero

## Qué es

Simulador financiero de Nodus construido con HTML, CSS y JavaScript vanilla. No usa frameworks ni servidor: se abre `index.html` en cualquier navegador.

Permite modificar clientes, precios, costos, equipo y capacidad, y ver al instante cómo cambian los ingresos, los márgenes, la utilidad, el punto de equilibrio y la necesidad de contratar.

## Cómo usarlo

1. Ajusta los supuestos en el panel izquierdo (secciones 1 a 5). Todo se recalcula automáticamente.
2. Usa los escenarios rápidos de la barra superior (grupos **Equipo** y **Cartera**, combinables) para comparar situaciones típicas.
3. Lee los 8 indicadores de la parte superior y la "Lectura rápida" de la pestaña **Resumen**.
4. Recorre las pestañas para analizar el negocio desde distintos ángulos.
5. Guarda la configuración en el navegador, o expórtala/impórtala como archivo `.json` para compartirla.

## Pestañas

### Resumen

- Lectura rápida en lenguaje claro del estado actual.
- Estado de resultados mensual: ingresos, costos directos, margen bruto, gastos operativos, resultado operativo, impuestos y utilidad neta.
- Indicadores clave con su fórmula.
- Gráficos: ingresos por segmento y a dónde se va el dinero.

### Escenarios y sensibilidad

- Resultado operativo según número de clientes, con el punto de equilibrio marcado.
- Utilidad neta con la misma cantidad de clientes pero distinta composición de cartera.
- Matriz de sensibilidad: utilidad neta según total de clientes y tipo de cliente.
- Sensibilidad a la TRM (±10% y ±20%).

### Capacidad y contratación

- Horas disponibles del equipo vs horas demandadas por los clientes.
- Utilización, clientes máximos antes de contratar y personas adicionales necesarias.
- Regla de contratación: contribución que habilitaría una persona nueva vs su costo.

### Economía por cliente

- Ingreso, costo directo, contribución, margen y contribución por hora de cada tipo de cliente.
- Punto de equilibrio por tipo de cliente.
- Detalle de cómo se calcula el costo de contenido.

### Adquisición y caja

- CAC, LTV, churn, vida media del cliente, payback y LTV/CAC.
- Cartera por cobrar, colchón de 3 meses de gastos fijos y caja mínima sugerida.

### Guía

- Cómo usar el simulador.
- Qué significa cada margen.
- De dónde sale cada número.
- Salario vs utilidad vs reinversión.
- Advertencias y alcance.

## Variables editables

### Supuestos generales

- TRM (COP por USD).
- Impuesto sobre utilidad (%), solo para simulación.
- Reserva / contingencia (% de ingresos).
- Meta de clientes.

### Clientes (segmentos)

- Nombre del segmento.
- Cantidad de clientes activos.
- Ticket por cliente (COP o USD según la moneda del segmento).
- Moneda (COP o USD).
- Costo del editor de contenido en USD por cliente/mes.
- Aporte del cliente a ese costo en USD por cliente/mes.
- Horas que consume cada cliente al mes.

### Equipo

- Nombre / rol.
- Costo empresa mensual (salario + prestaciones y cargas).
- Horas al mes.
- Horas al mes dedicadas a clientes.

### Tecnología y otros gastos

- Listas editables de herramientas y gastos fijos mensuales.

### Adquisición y caja

- Clientes nuevos por mes.
- Gasto de adquisición mensual.
- Churn mensual (%).
- Días de cobro y de pago.
- Caja disponible hoy.

## Fórmulas principales

```text
Ingreso por cliente COP   = ticket (si es USD: ticket × TRM)
Costo directo por cliente = (costo editor USD − aporte del cliente USD) × TRM
Contribución por cliente  = ingreso − costo directo
Ingresos                  = Σ clientes × ingreso por cliente
Costos directos           = Σ clientes × costo directo por cliente
Margen bruto              = ingresos − costos directos
Gastos operativos         = nómina + tecnología + otros gastos + reserva (% ingresos)
Resultado operativo       = margen bruto − gastos operativos
Impuesto modelado         = max(0, resultado operativo) × tasa
Utilidad neta             = resultado operativo − impuesto
Punto de equilibrio       = gastos fijos ÷ contribución promedio por cliente
Horas disponibles         = Σ horas a clientes del equipo
Horas demandadas          = Σ clientes × horas por cliente
Utilización               = horas demandadas ÷ horas disponibles
Clientes máximos          = horas disponibles ÷ horas promedio por cliente
CAC                       = gasto de adquisición ÷ clientes nuevos
Vida media del cliente    = 100 ÷ churn (% mensual)
LTV                       = contribución promedio × vida media
Payback                   = CAC ÷ contribución promedio
Cartera por cobrar        = ingresos × días de cobro ÷ 30
Caja mínima sugerida      = 3 × gastos fijos + cartera por cobrar
```

## Escenario inicial

- TRM: COP $3.200 / USD.
- 8 clientes Colombia a COP $4.000.000/mes.
- Segmento internacional de referencia: USD $2.000/mes, con 0 clientes.
- Editor: USD $400 por cliente/mes; el cliente aporta USD $200; Nodus subsidia USD $200 (COP $640.000 con TRM $3.200).
- 4 socios + 1 asistente, costo empresa COP $3.000.000 cada uno.
- Socios: 78 h/mes a clientes (160 h × 65% de dedicación a Nodus × 75% de ese tiempo a clientes).
- Clientes: 35 h/mes cada uno.
- Tecnología: Google Workspace, Claude, Google AI, Hostinger VPS/n8n, Vercel, Dominio (COP $338.000).
- Otros gastos: marketing COP $700.000 + legal/contabilidad COP $500.000.

Resultado del escenario base: ingresos $32.000.000, margen bruto 84%, resultado operativo $10.342.000, punto de equilibrio 5 clientes.

## Escenarios rápidos

La barra superior agrupa los atajos por tipo de cambio y cada grupo es independiente de los demás (se pueden combinar). El botón que coincide con el estado actual queda resaltado en azul, y se actualiza automáticamente si editas valores a mano:

### Grupo Equipo

Cambian solo el costo empresa de las personas cuyo nombre contiene "Socio". No tocan los clientes.

- **Socios a $3M**: etapa inicial.
- **Socios a $6M**: etapa consolidada (equivale a ~$5M de salario + cargas).

### Grupo Cartera de clientes

Cambian solo cuántos clientes hay en cada tipo de segmento (identificado por moneda). No tocan el equipo.

- **8 Colombia**: 8 clientes en el segmento COP.
- **4 Col + 4 Intl**: 4 clientes en COP + 4 en USD.
- **8 internacionales**: 8 clientes en el segmento USD.

Si el segmento necesario no existe, se crea automáticamente con los valores de referencia.

### Grupo Todo

- **Restaurar base**: vuelve a todos los valores del informe (8 clientes Colombia, socios a $3M, TRM $3.200, herramientas y gastos iniciales). No borra nada guardado.

## Qué costos NO deben cargarse a Nodus

Si el cliente paga directamente una herramienta (pauta, dominio, hosting, CRM, ManyChat, WhatsApp API, créditos de IA, etc.), no debe registrarse como costo de Nodus. Solo entra la parte que Nodus asume o subsidia.

## Guardado

- `Guardar` y `Cargar` usan `localStorage` del navegador (nada se envía a un servidor).
- `Borrar guardado` elimina la configuración almacenada en el navegador, pero no cambia los valores que están en pantalla.
- `Exportar` descarga un `.json` y `Importar` lo recupera, para compartir escenarios entre socios o equipos.

## Limitaciones

Todavía no se modelan:

- IVA y retenciones.
- Tributación real según estructura jurídica.
- Prestaciones calculadas automáticamente (se ingresan como costo empresa).
- Proyección mensual a 36 meses.
- Flujo de caja detallado mes a mes.
- Escenarios probabilísticos.
- Contratación automática según capacidad (el modelo la sugiere, no la aplica sola).

## Principio financiero

Nodus no se analiza por horas vendidas. Las horas sirven internamente para medir capacidad, carga, necesidad de contratación y rentabilidad por cliente.

El modelo conecta: `Clientes → ingresos → costos → capacidad → equipo → utilidad → reinversión → crecimiento`.

## Nota

Los valores iniciales son supuestos de planeación. Antes de tomar decisiones contables, tributarias, laborales o societarias, valídalos con un contador/asesor tributario y con la estructura jurídica concreta de Nodus.
