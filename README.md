# 📱 ConnectaTel: Análisis Exploratorio y Segmentación de Usuarios (Proyecto 6)

Este proyecto realiza la limpieza, procesamiento, análisis exploratorio de datos (EDA) y segmentación del comportamiento de clientes y uso de la red móvil para la empresa de telecomunicaciones **ConnectaTel**.

---

## 🎯 Objetivo del Proyecto

El objetivo principal es procesar los datos históricos de consumo y perfil demográfico de los clientes de ConnectaTel para:
1. Detectar e imputar inconsistencias en los datos (valores centinela, nulos y fechas fuera de rango).
2. Agrupar la actividad histórica de consumo (llamadas y mensajes de texto) a nivel usuario.
3. Evaluar la distribución del uso de la red e identificar patrones atípicos (*outliers*).
4. Segmentar la cartera de clientes por comportamiento de uso y rangos de edad para generar **insights comerciales ejecutivos**.

---

## 📊 Datasets Utilizados

El análisis integra **3 fuentes de datos CSV** ubicadas en la carpeta `/datasets/`:

1. **`users.csv` (Perfil de Clientes):**
   * `user_id`: Identificador único del cliente.
   * `age`: Edad del usuario (se corrigió el centinela `-999` reemplazándolo por la mediana).
   * `city`: Ciudad de residencia (se limpió el centinela `"?"` asignando `pd.NA`).
   * `plan`: Tipo de plan contratado (`Surf` o `Ultimate`).
   * `reg_date`: Fecha de registro/alta (se convirtieron las fechas a `datetime` y se filtraron inconsistencias del año 2026).

2. **`usage.csv` (Consumo Histórico):**
   * `user_id`: Identificador del cliente.
   * `type`: Tipo de servicio (`call` para llamadas o `text` para mensajes).
   * `duration`: Duración de la llamada en minutos (presenta nulos tipo MAR para mensajes de texto).
   * `length`: Cantidad de caracteres del mensaje de texto (presenta nulos tipo MAR para llamadas).
   * `date`: Fecha de la transacción.

3. **`plans.csv` (Tarifas y Términos del Servicio):**
   * `plan_name`: Nombre del plan (`Surf` o `Ultimate`).
   * `usd_monthly_pay`: Pago mensual en dólares.
   * `minutes_included`, `messages_included`: Cuotas incluidas de minutos y SMS por plan.

---

## 🛠️ Etapas del Análisis

El desarrollo sigue un pipeline estructurado en 8 pasos principales:
1. **Paso 1: Carga e Inspección Inicial:** Lectura de los 3 archivos CSV (`users.csv`, `usage.csv`, `plans.csv`) e inspección de estructuras y tipos de datos.
2. **Paso 2: Diagnóstico de Nulos y Mecanismo MAR:** Identificación de patrones *Missing At Random* (nulos de duración en SMS y nulos de caracteres en llamadas).
3. **Paso 3: Limpieza y Corrección de Fechas:** Tratamiento de centinelas (`-999`, `"?"`), estandarización a `datetime64` y depuración de registros con fechas inconsistentes (post-2024).
4. **Paso 4: Consolidación (`user_profile`):** Agregación del consumo por `user_id` (`cant_mensajes`, `cant_llamadas`, `cant_minutos_llamada`) y unión (*left merge*) con la tabla `users`.
5. **Paso 5: Visualización de Distribuciones:** Generación de histogramas comparativos por tipo de tarifa (`hue='plan'`) para evaluar las dinámicas de uso.
6. **Paso 6: Detección y Tratamiento de Outliers:** Inspección de valores atípicos mediante diagramas de caja (*Boxplots*) y cálculo de límites superiores con el método IQR.
7. **Paso 7: Segmentación de Clientes:** Clasificación categórica de la cartera por intensidad de consumo (`grupo_uso`) y rangos demográficos (`grupo_edad`).
8. **Paso 8: Informe Ejecutivo para Stakeholders:** Conclusiones estratégicas, análisis de calidad de datos y recomendaciones comerciales para ConnectaTel.
