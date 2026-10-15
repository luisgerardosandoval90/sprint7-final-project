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
1. **Paso 1 & 2: Carga e Inspección Inicial:** Inspección de estructuras, revisión de tipo de variables en las 3 fuentes de datos y verificación de nulos tipo MAR (*Missing At Random*).
2. **Paso 3: Limpieza de Datos:** Tratamiento de centinelas (`-999`, `"?"`), estandarización a `datetime64` y depuración de fechas fuera de alcance (post-2024).
3. **Paso 4: Consolidación (`user_profile`):** Agregación de consumo por `user_id` (`cant_mensajes`, `cant_llamadas`, `cant_minutos_llamada`) y unión (*left merge*) con la tabla `users`.
4. **Paso 5: Visualización y Outliers:** Gráficos de distribución (histogramas con `hue='plan'`) y evaluación de límites mediante el método IQR en diagramas de caja (*Boxplots*).
5. **Paso 6: Segmentación:** Clasificación multivariable en categorías de comportamiento (`grupo_uso`: Bajo, Medio, Alto) y grupos demográficos (`grupo_edad`: Joven, Adulto, Adulto Mayor).
6. **Paso 7: Informe Ejecutivo:** Conclusiones estratégicas sobre la retención, rediseño de oferta de planes y oportunidades de *Up-selling* para superusuarios.
